# WC Survey Cam — iOS / Safari version

Same `index.html` as the Android app. One file, two runtimes: it uses the
native plugins when they exist and falls back to browser APIs when they
don't, so there is no second codebase to keep in step.

## Files

```
index.html              the app
manifest.webmanifest    makes it installable
icon-180.png            home screen icon (iOS)
icon-192.png            home screen icon (Android/Chrome)
icon-512.png            splash / store icon
```

All four must sit in the same folder.

## Hosting

**It must be served over HTTPS.** iOS refuses the compass and the share sheet
on plain HTTP. Opening the file from Files or an Egnyte link will not work —
it has to be a real https:// address.

GitHub Pages is the least effort given the workflows already in use:

1. New repo, e.g. `wc-survey-cam-web` (private is fine — Pages can still
   serve it on a paid plan; otherwise use a public repo, there is nothing
   sensitive in here).
2. Drop these four files in the root.
3. Settings → Pages → Source: `main`, folder `/ (root)`.
4. It appears at `https://<user>.github.io/wc-survey-cam-web/` in a minute or two.

Send that link to the two iOS users.

## Installing on the iPhone

Safari → open the link → Share → **Add to Home Screen**.

Worth doing rather than just bookmarking: it runs full screen without the
Safari toolbars, and an installed web app's stored settings survive far
longer than a plain tab's (Safari clears data for sites not visited in seven
days — that would wipe the remembered project number).

## What differs from the Android app

| | Android app | iPhone in Safari |
|---|---|---|
| Compass | Rotation vector / magnetometer, declination applied | `webkitCompassHeading`, already true north |
| First use | — | First tap of **Lock Heading** asks permission, second locks |
| Camera | In-app viewfinder, one tap | Apple's camera, with its own "Use Photo" confirm |
| Zoom | Pinch in the viewfinder | Apple's own zoom |
| Save / Send | Two separate buttons | One **Save / Send** button → share sheet |
| Camera roll | Direct write | "Save Image" in the share sheet |
| Egnyte | Remembered target, one tap | Pick Egnyte in the share sheet each time |

The manual north dial and No Arrow work identically.

## Things to know

- **The confirm step is back on iOS.** Apple's file-input camera always shows
  "Retake / Use Photo". There is no way around it from a web page; removing
  it on Android needed a native viewfinder.
- **Verify the heading against a known bearing before trusting it.** iOS
  reports the compass differently depending on how the phone is held, and
  this has not been checked against a real instrument. Shoot a wall you know
  the bearing of and compare. If it reads consistently off by 90°, tell me and
  it is a one-line change.
- **No offline support.** Deliberate — service workers have caused silent
  stale-content failures in these apps before, and a cached-but-wrong app in
  the field is worse than one that plainly needs signal.
- **Nothing is stored on the phone.** Each photo goes straight to the share
  sheet. If a share is cancelled, that photo is gone — the same trade the
  Android build makes.
