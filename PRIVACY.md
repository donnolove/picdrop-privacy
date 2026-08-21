# PicDrop Privacy

PicDrop runs locally in the user's browser. It does not include analytics, advertising, accounts, a backend service, or third-party APIs.

## Data handled

- Image URLs are read from the webpage only when needed to resolve and download an image.
- Download preferences and custom categories are stored with `chrome.storage.sync`. Chrome may synchronize these settings between browsers signed into the same Google account.
- Downloads are initiated through the Chrome Downloads API.

PicDrop does not transmit browsing history, downloaded images, image URLs, or settings to a server operated by PicDrop.

## Permissions

- `downloads` starts image downloads and supplies a relative filename.
- `storage` saves the configured download folder, prompt preference, and up to ten custom categories.
- Static content-script match patterns allow PicDrop to detect images on HTTP and HTTPS pages.

## Website limitations

PicDrop does not bypass authentication, access controls, paywalls, DRM, or website permissions. Chrome prevents content scripts from running on protected browser pages and the Chrome Web Store.

## Changes

Privacy-impacting changes should update this document and be described in the release notes.
