# Project Overview

This project provides a minimalist, client-side web application for base64-encoding a file. Upload or drag in a file and the page prints the same string you would get from piping that file through the `base64` command line tool. It serves as a straightforward utility for anyone needing a base64 blob without shelling out or pasting sensitive contents into a third-party service.

## Architecture

The application is a single-page web utility implemented entirely within `index.html`. It operates as a static file, executing all its logic directly within the user's web browser. There is no backend server component, database, or complex build process involved — the selected file is read with the `FileReader` API and never leaves the machine.

Files are read as an `ArrayBuffer` rather than as text, so the output matches `base64` byte for byte regardless of the file's character encoding. Encoding is done with `btoa()` over 32KB chunks of the byte array, which avoids the call stack limits you hit passing a large array to `String.fromCharCode`.

By default the output is a single unwrapped line, matching the macOS/BSD `base64`. A toggle wraps it at 76 columns to match GNU coreutils.

## Key Files

*   `index.html`: This is the sole application file. It contains all the necessary HTML structure for the user interface, any embedded CSS for styling, and the JavaScript logic responsible for reading, encoding, and displaying the file.
*   `README.md`: Provides a concise overview of the project's purpose.
*   `CNAME`: Specifies a custom domain name for hosting the application, typically used in conjunction with GitHub Pages.

## How to Run

To run this application locally, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/calvinbrown085/base64.git
    ```
2.  **Navigate into the project directory:**
    ```bash
    cd base64
    ```
3.  **Open in a browser:**
    Simply open the `index.html` file directly in your preferred web browser.
    Alternatively, for a more standard local serving environment, you can use a simple static file server:
    *   **Using Python (if installed):**
        ```bash
        python -m http.server 8000
        ```
        Then, navigate to `http://localhost:8000` in your browser.
    *   **Using Node.js `serve` (if installed globally):**
        ```bash
        npx serve .
        ```
        Then, navigate to the URL provided by `serve`.

    Note that the Clipboard API only works in a secure context, so over plain `http://localhost` the copy button falls back to `document.execCommand('copy')`.

## How to Test

No automated test suite is provided with this repository. The application can be manually tested by:

1.  Opening `index.html` in a web browser.
2.  Dropping in a file, or clicking the drop zone and choosing one.
3.  Comparing the displayed string against the command line:
    ```bash
    cat nacha.txt | base64
    ```
    The two should match exactly. With the "Wrap at 76 characters" box checked, compare against a GNU coreutils `base64` instead (`gbase64` on macOS via Homebrew).
