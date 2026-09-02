# Tanmira Chat Google Drive Media

Tanmira Chat uses Google Identity Services + Google Drive API directly in the browser. Files are uploaded to the currently connected user's Google Drive, then shared as `Anyone with the link` viewer access. Firestore stores only media metadata and the share URL.

## One-time Google Cloud setup

1. Open Google Cloud Console for the Firebase project used by Tanmira Chat.
2. Enable **Google Drive API**.
3. Configure **Google Auth Platform / OAuth consent screen**.
4. Add the Drive scope used by the app:
   - `https://www.googleapis.com/auth/drive.file`
5. Create an **OAuth 2.0 Client ID** with application type **Web application**.
6. Add this Authorized JavaScript origin:
   - `https://iammdtanvirrahman2007.github.io`
7. Copy the Web Client ID. Web client IDs are public identifiers; do not paste a client secret into the repository.
8. Open `js/drive-config.js` and replace:
   - `PASTE_YOUR_WEB_CLIENT_ID.apps.googleusercontent.com`
   with the new Client ID.

## How the feature works

- User opens **🖼️ Media**.
- User taps **Connect Google Drive**.
- Google authorization is requested only for Drive files used by this app.
- The Google email is linked to the current Tanmira nickname in `users/{uid}`.
- Selecting a file uploads it to that user's Drive using a resumable upload.
- The file is changed to `Anyone with the link` + `Viewer` permission.
- Firestore receives the file ID, share URL, preview URL (images), download URL, filename, MIME type and size.
- Chat renders an image preview for images. Other files get a file card and Download button.

## Notes

- Google Drive remains the actual storage. Firebase Storage is not used.
- The browser never receives or stores a Google client secret.
- Google access tokens are kept in memory for the current page session, not written to Firestore.
- Because the chosen sharing model is public-link viewing, anyone who obtains the share URL can access that file. The URL is hidden from the normal chat UI, but it is still the underlying public URL.
- The current implementation uses `drive.file`, which is a narrow, non-sensitive Drive scope intended for files created/used by the app.
