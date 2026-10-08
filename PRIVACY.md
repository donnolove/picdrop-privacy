# PicDrop Privacy

Applies to PicDrop **1.4.2**. Updated October 8, 2026.

PicDrop runs locally in the user's browser. It does not include analytics, advertising, accounts, a backend service, or third-party APIs.

## Data handled

- Image URLs are read from the webpage only when needed to resolve and download an image.
- While the download toolbar is active, PicDrop tracks the selected image component and rechecks its source before a click starts a download. Target references and source snapshots are held in memory and are not saved to extension settings.
- The extension's enabled state, download folder, custom categories, toolbar position, and UI collision avoidance setting are stored with `chrome.storage.sync`. Chrome may synchronize these settings between browsers signed into the same Google account. Custom icon images are stored separately and do not synchronize with these settings.
- Custom icon images uploaded by the user are resized to 96 px and stored only on this device with `chrome.storage.local`. They are not synchronized or uploaded anywhere.
- Settings backups are created only when the user clicks "Export settings". The `.json` file contains the saved download folder, categories, toolbar position, UI collision avoidance setting, and custom icons. Unsaved edits and the extension's overall enabled state are excluded. The file is saved to the user's computer and is never uploaded by PicDrop. Importing reads only the file the user selects and asks for confirmation before replacing the settings and custom icons covered by the backup. A backup can transfer custom icons to another computer.
- Downloads are initiated through the Chrome Downloads API.
- For HTTP/HTTPS image URLs on the current page's origin without a recognizable image extension or format parameter, a click can retrieve the image in the content script using browser-managed credentials. Redirects are rejected. PicDrop does not read cookies, passwords, or authentication tokens. The response must have a supported image MIME type and decode as an image before it is downloaded.
- Retrieved image bytes and their source URL are temporarily passed to the extension service worker for a local download. The source URL supplies the filename basename; the response image MIME type supplies the extension. PicDrop does not read the response's `Content-Disposition` filename or infer a name from query parameters. These bytes and URLs are not stored in extension settings or uploaded to another service.

PicDrop does not transmit browsing history, downloaded images, image URLs, or settings to a server operated by PicDrop.

## Permissions

- `downloads` starts image downloads and supplies a relative filename.
- `storage` saves the extension's enabled state, configured download folder, toolbar position, UI collision avoidance setting, up to ten custom categories, and up to twenty custom icons. PicDrop does not offer a Save As prompt preference; image downloads use `saveAs: false`.
- Static content-script match patterns allow PicDrop to detect images on HTTP and HTTPS pages.

## Website limitations

PicDrop does not bypass authentication, access controls, paywalls, DRM, or website permissions. Chrome prevents content scripts from running on protected browser pages and the Chrome Web Store.

Page retrieval is limited to 5 MiB (5,242,880 bytes) of image data and a 15-second retrieval and validation timeout. It rejects all redirects, including same-origin redirects, and fails without falling back to a direct download if the response is a login page or invalid image. Website authentication, CSP, and request-origin checks may still prevent retrieval. These added limits do not apply to the existing direct URL download flow.

## Changes

Version 1.4.2 adds category ordering and nine toolbar positions. The saved category array preserves its order in settings backups. These changes use the existing permissions and data storage.

Version 1.4.1 adds active image tracking for changing galleries and same-origin page retrieval for image endpoints without a recognizable image extension or format parameter. It uses the existing permissions and adds no dependencies, cookie access permission, analytics, or external upload service.

Privacy-impacting changes should update this document and be described in the release notes.
