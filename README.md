# ClipSpark AI — Advanced Netlify starter

## What is included
- Responsive upload/link website and clip settings.
- Netlify serverless function that securely forwards jobs to a cloud video provider.
- Rights confirmation and clear YouTube-link limitation.
- Cloud processing design to avoid running FFmpeg on your phone.

## Important: this is a starter, not a finished AI video service
The website and API bridge are included. A compatible cloud provider, its API credentials, video storage and rendering service are NOT included. Until configured, the site will show that cloud processing is not connected. Do not mistake the interface for working AI processing.

A normal YouTube watch/share URL cannot be downloaded by this static Netlify site. YouTube Data API does not provide the underlying video file. Even when a creator permits clipping, technical access must come from an authorized provider workflow or a video file supplied by the creator.

## Deploy
1. Extract the ZIP.
2. Put the folder in a Git repository and connect it to Netlify (recommended; required for the function).
3. Netlify reads `netlify.toml`; publish directory is `public`, functions directory is `netlify/functions`.
4. In Netlify: Site configuration → Environment variables, add:
   - `CLIPSPARK_API_URL` = provider job-creation endpoint
   - `CLIPSPARK_API_KEY` = private API key
5. Redeploy.

Netlify Drop is fine for a static preview, but use a normal project deployment connected to the repository for the serverless function and environment variables.

## Provider contract
The function forwards the browser request to `CLIPSPARK_API_URL` with `Authorization: Bearer <key>`. The provider must accept the forwarded multipart request and return JSON, for example:
`{"jobId":"job_123","message":"Queued","clips":[{"title":"Highlight 1","url":"https://provider.example/clip.mp4","duration":"00:42"}]}`

For large videos, adapt the frontend to the provider's signed direct-upload flow. Passing large binary videos through a serverless function can exceed request limits/timeouts. Add job-status polling, authentication, rate limits and upload-size limits before making the service public.

Only submit videos you own or have permission and rights to use. Never expose API keys in `public/index.html`.
