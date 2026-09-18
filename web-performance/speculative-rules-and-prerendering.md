# Web Performance: Speculation Rules API and Instant Navigation

## The Limits of Preload and Prefetch
Traditional `<link rel="prefetch">` fetches HTML documents into the HTTP cache, reducing download time upon click.
However, it does not execute JavaScript, parse CSS, or construct DOM trees:
- Time to Interactive (TTI) still requires full script evaluation and subresource fetching.

## Speculation Rules API
The W3C Speculation Rules API enables true full prerendering:
```html
<script type="speculationrules">
{
  "prerender": [
    {
      "source": "list",
      "urls": ["/dashboard", "/profile"],
      "eagerness": "moderate"
    }
  ]
}
</script>
```

## Prerendering Execution Model
1. Browser instantiates an invisible, sandboxed background renderer process.
2. The page loads, constructs DOM, fetches stylesheets, and executes initial render scripts.
3. Audio, video, and intrusive background timers are suspended until activation.
4. When the user clicks the navigation link, the background renderer swaps immediately to the foreground with 0ms Largest Contentful Paint (LCP).
