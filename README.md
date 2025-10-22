# ShareVolume

## Summary
ShareVolume is a lightweight, static web application designed to fetch and display "Shares Outstanding" data for public companies from the SEC EDGAR API. Users can view the maximum and minimum shares outstanding, along with their respective fiscal years, for a given CIK (Central Index Key). The application dynamically updates its content based on either a default company (Cintas) or a CIK provided in the URL query parameter, all without requiring a page reload.

## Live Demo
[Live Demo](https://shantanu-data.github.io/sharevolume/)

## Setup
This project is a static web application built with HTML, CSS (Tailwind CSS), and JavaScript. No backend server, database, or build tools are required. Simply open the `index.html` file in your web browser, or deploy it to a static hosting service like GitHub Pages.

## How to Use
1.  **Default View:** Open the live demo link provided above. By default, the application will fetch and display shares outstanding data for Cintas (CIK 0000723254).
2.  **Custom CIK:** To view data for a different company, append a `?CIK=` query parameter to the URL, followed by the 10-digit CIK of the desired company.
    *   **Example:** To see data for another company, you would navigate to:
        `https://shantanu-data.github.io/sharevolume/?CIK=0001018724` (replace `0001018724` with any valid 10-digit CIK).
    The page content (`title`, `h1`, and share data) will update dynamically without a full page reload.

## Code Explanation

The project consists of a single `index.html` file that encapsulates all the HTML structure, CSS styling, and JavaScript logic.

*   **HTML (`index.html`)**:
    *   Defines the basic page structure, including the `<head>` for metadata and the `<body>` for visible content.
    *   Includes a `<title>` tag and an `<h1>` element (`id="share-entity-name"`) to display the company's name, which are dynamically updated.
    *   Uses a `div` structure to create two distinct cards for displaying "Max Shares Outstanding" and "Min Shares Outstanding".
    *   Specific `id` attributes are assigned to elements that will be updated by JavaScript:
        *   `share-entity-name`: For the company's name in the `<h1>`.
        *   `share-max-value`, `share-max-fy`: For the maximum share value and its fiscal year.
        *   `share-min-value`, `share-min-fy`: For the minimum share value and its fiscal year.
    *   An `error-message` element is included to display any issues during data fetching.

*   **CSS (Inline with Tailwind CSS)**:
    *   Leverages **Tailwind CSS** via a CDN, providing a utility-first framework for rapid styling and responsive design. This handles most of the layout, typography, spacing, and responsive behaviors.
    *   Includes a small `<style>` block for custom CSS rules. These rules primarily define `body` background/text colors, limit the `container-wrapper` width, and provide specific styling for `.card` elements, `.value-label` text, and `.share-value` figures (e.g., larger font, bolding, specific accent colors).

*   **JavaScript (Embedded in `index.html`)**:
    *   **Constants**: Defines `SEC_USER_AGENT` (as required by SEC guidance) and `DEFAULT_CIK`.
    *   **DOM Element References**: Stores references to all dynamically updated HTML elements (e.g., `entityNameEl`, `maxValEl`) for efficient manipulation.
    *   **`resetDisplay()` function**: Clears existing data and shows loading indicators, useful before fetching new data.
    *   **`fetchAndRenderCompanyData(cik)` function**:
        *   Constructs the SEC API URL using the provided CIK.
        *   Sends an asynchronous `fetch` request to the SEC endpoint, including the `User-Agent` header.
        *   Parses the JSON response.
        *   Filters the `shares` data:
            *   Only includes entries where `fy` (fiscal year) is greater than "2020".
            *   Ensures `val` (value) is a numeric type.
        *   Iterates through the filtered shares to determine the `max` and `min` values along with their corresponding fiscal years.
        *   Updates the `<h1>`, `<title>`, and all relevant `<span>` elements (`share-max-value`, `share-max-fy`, `share-min-value`, `share-min-fy`) with the processed data.
        *   Includes comprehensive error handling to catch API issues, network errors, or invalid data, displaying user-friendly messages.
    *   **Initialization**: An event listener for `DOMContentLoaded` ensures that the script runs only after the HTML is fully loaded. It checks for a `CIK` query parameter in the URL. If present, it uses that CIK; otherwise, it falls back to the `DEFAULT_CIK` for Cintas. Finally, it calls `fetchAndRenderCompanyData()` to kickstart the data loading process.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.