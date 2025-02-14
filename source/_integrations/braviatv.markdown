---
title: Sony Bravia TV
description: Instructions on how to integrate a Sony Bravia TV into Home Assistant.
ha_category:
  - Button
  - Media player
  - Remote
ha_release: 0.23
ha_iot_class: Local Polling
ha_codeowners:
  - '@bieniu'
  - '@Drafteed'
ha_domain: braviatv
ha_config_flow: true
ha_platforms:
  - button
  - media_player
  - remote
ha_ssdp: true
ha_integration_type: device
---

The **Bravia TV** {% term integration %} allows you to control a [Sony Bravia TV](https://www.sony.com/).

Almost all [Sony Bravia TV 2013 and newer](https://info.tvsideview.sony.net/en_ww/home_device.html#bravia) are supported. For older TVs see more generic methods to control your device [below](#for-tvs-older-than-2013).

{% include integrations/config_flow.md %}

## Authentication

The Bravia TV integration supports two types of authentication:

- **PSK (Pre-Shared-Key)** is a user-defined secret key used for access control. This authentication method is recommended as more reliable and stable. To set up and enable PSK on your TV, go to: **Settings -> Network -> Home Network Setup -> IP Control**.
- **PIN Code** authentication is easier and does not require additional settings.

For more information, see [IP Control Authentication](https://pro-bravia.sony.net/develop/integrate/ip-control/index.html#ip-control-authentication).

## Common issues

### TV does not generate new pin

If you have previously set up your TV with any Home Assistant instances via PIN code, you must remove Home Assistant from your TV in order for your TV to generate a new pin. To do this, you must do **one** of the following:

- On your TV, go to: **Settings** > **Network** > **Remote device settings** > **Deregister remote device**. Disable and re-enable the **Control remotely** after. Menu titles may differ slightly between models. If needed, refer to your specific model's [manual](https://www.sony.com/electronics/support/manuals) for additional guidance.
- Reset your TV to factory condition.

## Media browser

Using the media browser, you can view a list of all installed applications and TV channels and launch them.

## Play media service

The `play_media` {% term service %} can be used in a automation or script to switch to a specified application or TV channel. It selects the best matching application or channel according to the `media_content_id`:

 1. Channel number *(i.e., '1' or '6')*
 2. Exact app or channel name *(i.e., 'Google Play' or 'CNN')*
 3. Substring in app or channel name *(i.e., 'BFM' in 'BFM TV')*
 4. URI-string of app or channel *(i.e., 'tv:dvbt?trip=9999.441.41104&srvName=BBC HD')*

**Example to open YouTube app:**

```yaml
service: media_player.play_media
target:
  entity_id: media_player.bravia_tv
data:
  media_content_id: "YouTube"
  media_content_type: "app"
```

**Example to switch to channel number 11:**

```yaml
service: media_player.play_media
target:
  entity_id: media_player.bravia_tv
data:
  media_content_id: 11
  media_content_type: "channel"
```

**Example to switch to channel including 'news' in its name:**

```yaml
service: media_player.play_media
target:
  entity_id: media_player.bravia_tv
data:
  media_content_id: "news"
  media_content_type: "channel"
```

## Remote

The integration supports `remote` platform. The remote allows you to send key commands to your TV with the `remote.send_command` service.

The commands that can be sent to the TV depends on the model of your TV. To display a list of supported commands for your TV, call the service `remote.send_command` with non-valid command (e.g. `Test`). A list of available commands will be displayed in [Home Assistant System Logs](https://my.home-assistant.io/redirect/logs).

{% details "Some commonly used commands" %}

- Up
- Down
- Left
- Right
- Confirm
- Return
- Home
- Exit
- Rewind
- Forward
- ActionMenu
- SyncMenu
- Num0
- Num1
- Num2
- Num3
- Num4
- Num5
- Num6
- Num7
- Num8
- Num9

{% enddetails %}

**Example to send `Down` key command:**

```yaml
service: remote.send_command
target:
  entity_id: remote.bravia_tv
data:
  command: "Down"
```

## Buttons

The integration supports `button` platform and allows you to reboot the device or terminate all running applications.

## For TVs older than 2013

Users of TVs older than 2013 can control their devices using [HDMI-CEC](/integrations/hdmi_cec/), [Broadlink](/integrations/broadlink/) or [Kodi](/integrations/kodi/) integrations.
