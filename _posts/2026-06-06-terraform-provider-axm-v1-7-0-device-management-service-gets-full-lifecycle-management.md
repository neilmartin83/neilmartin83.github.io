---
layout: post
title: "terraform-provider-axm v1.7.1 - Device Management Service gets full IaC lifecycle management"
date: 2026-06-06
---

Yo Terraform massive! [v1.7.1](https://registry.terraform.io/providers/neilmartin83/axm/latest/docs) is here and it's a big one. Device Management Service creation, deletion, and a brand new resource for managing which service each device family enrols into by default. This marks a significant milestone; full Infrastructure as Code (IaC) lifecycle management for Device Management Services.

## [axm_device_management_service](https://registry.terraform.io/providers/neilmartin83/axm/latest/docs/resources/device_management_service) - now with a full lifecycle

Previously, `axm_device_management_service` was limited. You had to supply the server ID yourself (i.e. it already had to exist), it couldn't create or delete anything, and it just managed device serial number assignments against an existing server. From provider release 1.7.1, it's a proper resource backed by the [Apple Business API v2.1 API](https://developer.apple.com/documentation/applebusinessapi) endpoints.

```hcl
data "jamfpro_device_enrollments_public_key" "current" {}

resource "axm_device_management_service" "prod" {
  name = "Jamf Pro - Production"

  server_certificate = {
    name = "PublicKey.pem"
    data = base64encode(data.jamfpro_device_enrollments_public_key.current.public_key)
  }

  allow_release = false

  device_ids = [
    "SERIALNUM1",
    "SERIALNUM2",
  ]
}
```

`server_certificate` takes the base64-encoded public key you'd normally upload in the Apple Business web UI. This example demonstrates sourcing and base64-encoding the public key using the [Deployment Theory jamfpro Provider](https://registry.terraform.io/providers/deploymenttheory/jamfpro/latest/docs/data-sources/device_enrollments_public_key). You could use `filebase64("${path.module}/PublicKey.pem")` if you want to read it from a local file in the same module instead.

`allow_release` controls whether this service can release devices. `device_ids` is optional - leave it out if you'd rather not manage assignments through Terraform.

The gory gotchas:

- **Business scope only for create, update, and delete.** If you're running against Apple School Manager, the resource will refuse and tell you why. Import still works in either scope.
- **Destroy pre-flight.** Before deleting a server, the provider automatically clears any default product family assignments and unassigns all devices. Apple will return a 409 if you try to delete a server with either still attached, so both steps happen unconditionally on destroy.
- **Certificates aren't returned by the API.** Apple doesn't send the cert data back in GET responses, so what you write to config is what lives in state. Rotate the cert by updating `data` in config and applying.

## [axm_default_device_assignment](https://registry.terraform.io/providers/neilmartin83/axm/latest/docs/resources/default_device_assignment)

This is a new singleton resource representing the org-wide default device assignment page - what you set in Apple Business Manager under _Devices > Management Services > Default Device Assignment_ to control which Device Management Service newly enrolled devices go to, by device family. The example below sets the `prod` Device Management Service as the default for all device families.

Note that `watch` is available here (as the API implements it) but it is not shown in the Apple Business web UI for me at the time of writing.

```hcl
data "jamfpro_device_enrollments_public_key" "current" {}

resource "axm_device_management_service" "prod" {
  name = "Jamf Pro - Production"

  server_certificate = {
    name = "PublicKey.pem"
    data = base64encode(data.jamfpro_device_enrollments_public_key.current.public_key)
  }

  allow_release = false

  device_ids = [
    "SERIALNUM1",
    "SERIALNUM2",
  ]
}

resource "axm_default_device_assignment" "org" {
  apple_tv         = axm_device_management_service.prod.id
  apple_vision_pro = axm_device_management_service.prod.id
  ipad             = axm_device_management_service.prod.id
  iphone           = axm_device_management_service.prod.id
  mac              = axm_device_management_service.prod.id
  watch            = axm_device_management_service.prod.id
}
```

Each attribute takes an Device Management Service ID. Set it to a server ID to assign that family, set it to `""` to ensure it's unassigned (from the server tracked in state), or omit it entirely to leave it unmanaged by Terraform.

A couple of things to know before you deploy this:

**If you already have defaults configured in Apple Business Manager**, set the attributes to match the current state on first apply rather than jumping straight to your desired end state. Terraform needs to know where a family is coming from before it can move it cleanly. Going in with an empty state and trying to move a family that's already assigned to an untracked server will earn you a 409 from Apple.

**Moving a family between servers** requires two API calls in the right order - Apple won't let you claim a family that's already assigned elsewhere. The provider handles this automatically: it clears the server losing the family first, then sets the server gaining it. You don't need to think about it.

## Upgrading

If you have existing `axm_device_management_service` resources, `id` is now Computed-only and `name` is Required. Run `terraform import axm_device_management_service.<name> <server-id>` to bring existing servers under management, then update your configs accordingly.

Full docs on the [Terraform Registry](https://registry.terraform.io/providers/neilmartin83/axm/latest/docs) and source on [GitHub](https://github.com/neilmartin83/terraform-provider-axm). As always, feedback, berating, issues and PRs are very welcome.

Still holding out for Device Management Service token creation and VPP token management in the API. The Business API keeps growing though, so I remain increasingly optimistic. One day (hopefully soon)... 🤞
