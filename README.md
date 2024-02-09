## A very simple http static file server

### Build

```bash
go build -o ./serve
# Copy the binary to a bin directory (assuming ~/.local/bin is in your PATH)
cp ./serve ~/.local/bin
```

### Usage

```
Usage: serve [options] [directory]

Options:
  --help              Print this help message.
  --loglevel <level>  The log level. Default: info
  --port <port>       The port to serve on. Default: 8080
  --redirect <url>    Redirect all unmatched routes to a specified url.

Hot Module Reload
  --aw           Alias for '--watch --auto-reload'
  --watch        When enabled, server will send fs events when files are changed. To listen to these add event listeners to `window.HMR` on client side.
  --auto-reload  Automatically inject a script to html files that will reload the page on a 'watch' change event.

Caching
  --maxage <seconds>   The max-age value to set in the Cache-Control header.
  --nocache            Disable caching.
  --noetag             Disable ETag generation.
  --cache:max <MB>     Maximum size of all files in the cache. Default: 100MB
  --cache:flimit <MB>  Maximum size of single file that can be put in cache. Default: 10MB
```

To serve files from the `public` directory of the current directory on port 8000:

```bash
serve --port 8000 ./public
```

### Auto-reload

When auto-reload is enabled, either via `--auto-reload` or `--aw` flag, a script tag will be injected to every ".html" file that will reload the page every time that file is changed. Note that the `--auto-reload` flag must be used alongside the `--watch` flag.

### Watch mode

When watch mode is enabled, either via `--watch` or `--aw` flag, to every ".html" file a script tag will be injected enabling listening to file changes within the hosted directory.

Listening to these changes can be done by adding event listeners to `window.HMR` object:

```javascript
HMR.onChange((event) => {
  console.log(`file ${event.file} has changed`);
});

HMR.onCreate((event) => {
  console.log(`file ${event.file} has been created`);
});

HMR.onDelete((event) => {
  console.log(`file ${event.file} has been deleted`);
});

HMR.onRename((event) => {
  console.log(`file ${event.oldFile} has been renamed to ${event.file}`);
});

HMR.onCurrentPageChange((event) => {
  console.log(`current page's file has changed`);
});
```
