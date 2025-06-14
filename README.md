# worker
🌐 A simple and fast Cloudflare Worker that acts as a proxy — bypass access restrictions and reach any website via your own custom Worker endpoint

## How It Works

This Worker takes any incoming request, changes the hostname to your desired target (like `example.com`), and forwards the request. It then returns the response from the target website.
You can use it to send request to unaccessible websites, specially it is suitable for telegram bot (api.telegram.org)
---

## 🚀 Getting Started

### 1. Create a New Worker on Cloudflare

- Go to [Cloudflare Workers Dashboard](https://dash.cloudflare.com/)
- Click **"Create a Worker"**

### 2. Replace the Default Code

Copy and paste the following code into the Worker editor:

```js
async function handleRequest(request) {
  const url = new URL(request.url);

  // Change the hostname to your target domain
  url.hostname = 'myservice.com'; // ← Replace this with your desired domain

  const newRequest = new Request(url.toString(), request);

  try {
    const response = await fetch(newRequest);
    return response;
  } catch (error) {
    return new Response('Internal Server Error', {
      status: 500,
      headers: {
        'content-type': 'text/plain',
      },
    });
  }
}

addEventListener('fetch', (event) => {
  event.respondWith(handleRequest(event.request));
});
```

### 3. Customize the Target Domain
Replace 'myservice.com' in the code with the domain you want to proxy traffic to.

```url.hostname = 'wikipedia.org';```

### 4. Deploy the Worker
Click “Save and Deploy” and Copy your Worker URL
Use it instead of the your service url.

---

## ⚠️ Notes

- This is a basic proxy. It doesn’t rewrite content or handle advanced request/response logic.
- No HTTPS validation or CORS headers are handled — you can extend the code if needed.
- Use responsibly and respect the terms of service of any proxied websites.
