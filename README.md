# chromedp-screenshots

A command-line tool for taking web page screenshots using [chromedp](https://github.com/chromedp/chromedp) (headless Chrome). Supports CSS selector-based element capture, full-page screenshots, multiple URLs in parallel, and Chrome profile reuse.

## Features

- **Viewport screenshot** – capture the visible area with configurable width, height, and scale factor
- **Element screenshot** – target a specific element via CSS selector (`-q`)
- **Full-page screenshot** – capture the entire scrollable page (`-f`)
- **Multiple URLs** – pass multiple `-u` flags to capture several pages in parallel (one Chrome process, separate tabs)
- **Chrome profile support** – reuse an existing Chrome profile for logged-in sessions (`-pd`)
- **Custom Chrome flags** – pass arbitrary Chrome flags with `-c`

## Requirements

- Go 1.23+
- Google Chrome / Chromium installed

## Installation

```bash
go install github.com/xshoji/chromedp-screenshots@latest
```

Or build from source:

```bash
git clone https://github.com/xshoji/chromedp-screenshots.git
cd chromedp-screenshots
go build -o chromedp-screenshots .
```

## Usage

```bash
go run main.go -u <URL> [options]
```

### Options

| Flag | Default | Description |
|------|---------|-------------|
| `-u` | *(required)* | URL to capture (can be specified multiple times) |
| `-q` | `""` | CSS selector – screenshot the first matching element |
| `-o` | `/tmp/img.png` | Output file path (auto-numbered with multiple URLs: `_001.png`, `_002.png`, …) |
| `-pd` | `""` | Chrome profile directory to copy and cache |
| `-w` | `3` | Wait seconds after navigation before capturing |
| `-wi` | `1280` | Viewport width |
| `-he` | `860` | Viewport height |
| `-ds` | `2.0` | Device scale factor (2.0 = Retina) |
| `-f` | `false` | Enable full-page screenshot |
| `-d` | `false` | Enable debug mode |
| `-n` | `false` | Disable headless mode (show browser window) |
| `-s` | `false` | Save cached profile (do not delete after execution) |
| `-c` | `""` | Extra Chrome flag as `key=value` (can be specified multiple times) |

### Examples

```bash
# Viewport screenshot
go run main.go -u="https://www.example.com/" -wi=1280 -he=800 -o=/tmp/example.png

# Element screenshot with CSS selector
go run main.go -u="https://news.yahoo.co.jp/" -q="#liveStream" -o="/tmp/livestream.png"

# Full-page screenshot
go run main.go -u="https://www.example.com/" -f -o=/tmp/fullpage.png

# Multiple URLs (parallel capture)
go run main.go -u="https://www.yahoo.co.jp/" -u="https://www.google.com/" -o=/tmp/sites.png

# With Chrome profile (for logged-in sessions)
go run main.go -u="https://example.com/dashboard" \
  -pd="/Users/you/Library/Application Support/Google/Chrome/Default" \
  -s -o=/tmp/dashboard.png

# Custom Chrome flags
go run main.go -u="https://example.com/" -c "lang=ja" -c "disable-extensions"
```

### Environment Variables

| Variable | Description |
|----------|-------------|
| `CHROMEDP_SCREENSHOT_CACHE_DIR` | Override the default profile cache directory (`~/.chromedpscreenshot`) |

## Development

### Build

```bash
go build -ldflags="-s -w" -trimpath -o cdpss main.go
```



## License

See [LICENSE](LICENSE) for details.
