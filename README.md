# Repro for issue 7500

## Versions

firebase-tools: v13.14.2<br>
node: v20.9.0<br>
platform: macOS Sonoma 14.5

## Steps to reproduce

1. Run `npm i`
1. Run `npm run build`
1. Run `firebase deploy --project PROJECT_ID`
1. Open "https://SITE_ID.web.app" to verify if deployed. Note: You might have to hard refresh.
   - Shows page not found

## Notes

Deploying using firebase-tools v13.11.2 works even when using `"./*"` in `hosting.ignore` array:

```json
{
  "hosting": {
    "public": "./dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**",
      "*.sh",
      "./*"
    ],
    "headers": [
      {
        "source": "**",
        "headers": [
          {
            "key": "Cache-Control",
            "value": "public, max-age=86000, s-maxage=86000"
          }
        ]
      }
    ],
    "rewrites": [
      { "source": "/image/**", "function": "api" },
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

Deploying using firebase-tools v13.14.2 works after removing `"./*"` from `hosting.ignore` array:

```json
{
  "hosting": {
    "public": "./dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**",
      "*.sh"
    ],
    "headers": [
      {
        "source": "**",
        "headers": [
          {
            "key": "Cache-Control",
            "value": "public, max-age=86000, s-maxage=86000"
          }
        ]
      }
    ],
    "rewrites": [
      { "source": "/image/**", "function": "api" },
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
