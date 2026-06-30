---
title: "Home page Customization"
description: "Shortcut tiles, automatic logout, PIN policy, redirects, and self-service."
lead: "Make the shared iPad fit your team."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ipad"
weight: 62330
toc: true
---

All optional. Add the blocks you want to the managed app config you push from Intune. For
type, allowed values, and default of every key, see [Configuration keys](../configuration_keys).

## Shortcut tiles

`shortcut_list` adds tiles to the home screen. Each tile is a dict with three keys:

- `title` — label under the tile.
- `url` — where it opens; accepts the [tokens below](#personalize-links-per-user).
- `iconName` — remote icon, fetched as `<name>.png` from your image host; unresolved name = no icon.

`teams`, `outlook`, `myapps` below are icons the host already serves.

```xml
<key>shortcut_list</key>
<array>
  <dict>
    <key>title</key>
    <string>Teams</string>
    <key>url</key>
    <string>msteams://</string>
    <key>iconName</key>
    <string>teams</string>
  </dict>
  <dict>
    <key>title</key>
    <string>Outlook</string>
    <key>url</key>
    <string>ms-outlook://</string>
    <key>iconName</key>
    <string>outlook</string>
  </dict>
  <dict>
    <key>title</key>
    <string>My Apps</string>
    <key>url</key>
    <string>https://myapps.microsoft.com</string>
    <key>iconName</key>
    <string>myapps</string>
  </dict>
</array>
```

## Personalize links per user

A tile or redirect URL can carry who's signed in. The app substitutes these tokens:

| Token            | Becomes           |
| ---------------- | ----------------- |
| `{email}`        | Email address     |
| `{username}`     | Username          |
| `{name}`         | Display name      |
| `{realm}`        | Realm             |
| `{organization}` | Organization      |
| `{tenantID}`     | Tenant ID         |

Drop a token into a URL:

```xml
<key>url</key>
<string>https://myapps.microsoft.com/?login_hint={email}</string>
```

## Automatic logout

`auto_logout` signs the iPad out after a set time. Use one of the buckets in
[Configuration keys](../configuration_keys). `never`, `one-time use`, and any unrecognized
value mean no timer.

```xml
<key>auto_logout</key>
<string>5m</string>
```

## Require a PIN or second factor

`authentication_type` — when the user proves themselves a second time:

| Value    | Prompts                           |
| -------- | --------------------------------- |
| `none`   | Never; sign-in is the only step   |
| `oninit` | Once, right after sign-in         |
| `onuse`  | Each time an app or tile needs it |

```xml
<key>authentication_type</key>
<string>oninit</string>
```

## Open an app or page automatically

The two URL keys accept the [tokens above](#personalize-links-per-user):

- `default_sp_for_auto_login` — auto-starts sign-in for one service provider by name.
- `open_url_after_login` — opens a URL right after sign-in.
- `open_url_after_logout` — opens a URL right after logout.

```xml
<key>open_url_after_login</key>
<string>https://myapps.microsoft.com/?login_hint={email}</string>
```

## Self-service for users who aren't enrolled yet

`self_service_url` — where an unenrolled user goes to enroll. Build the workflow in the
IDmelon Admin Panel, then paste its URL here.

```xml
<key>self_service_url</key>
<string>https://your-self-service-workflow-url</string>
```

## In-app browser vs Safari

`use_internal_browser` — on, links open in the in-app browser; off, links hand to Safari.
See the default in [Configuration keys](../configuration_keys).

```xml
<key>use_internal_browser</key>
<true/>
```

## Where this fits

Each block goes inside the same `<dict>` from
[Set up a shared iPad](../set_up_a_shared_ipad). Add the keys you want, push, and sync.
