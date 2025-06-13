<!-- From critical phase -->
# Project: Galvanizers - WordPress Website

This document provides a comprehensive overview of the Galvanizers WordPress project, based on a static analysis of its codebase.

## 1. Project Overview

This project is a WordPress website, likely built for a company in the industrial or manufacturing sector, specifically related to galvanizing processes. The site appears to be structured as a **WordPress Multisite** installation, designed to manage and share content across multiple related sites.

The primary purpose is to serve as a corporate or informational website, showcasing case studies, publications, and company information. The presence of development and testing tools suggests the analyzed codebase is from a **development or staging environment**.

### Key Technologies

*   **CMS:** WordPress
*   **PHP Version:** 7.4+ (as per `composer.json`)
*   **Templating Engine:** [Timber](https://www.upstatement.com/timber/) with Twig templates. This promotes a clean separation of logic (PHP) and presentation (HTML/Twig).
*   **JavaScript:** Primarily uses jQuery, with some custom scripts for specific functionalities.
*   **CSS:** Utilizes SASS for organized and maintainable styling.
*   **Dependency Management:** PHP dependencies are managed via Composer.

## 2. Core Functionality & Features

*   **Content Syndication:** The `threewp-broadcast` plugin is a core component, indicating that content (posts, pages, etc.) is likely created on a primary site and then "broadcast" or copied to other sites within the network.
*   **Custom Content Types:** The site uses custom post types for "Case Studies", "Publications", and "Galvanizers", allowing for structured and specific content management.
*   **Advanced Custom Fields (ACF):** The presence of the ACF plugin suggests that custom fields are heavily used to structure the content within posts and pages, providing a more flexible content editing experience than the standard WordPress editor.
*   **SEO Optimization:** The site uses the "Rank Math SEO" plugin for search engine optimization. An additional plugin, "ACF Content Analysis for Yoast SEO," is also present, which might indicate a transition from Yoast to Rank Math or a specific need for ACF integration that Rank Math might not cover.
*   **Security:** The "Wordfence" plugin is installed and active, providing a web application firewall (WAF) and security scanning capabilities.
*   **User Experience:**
    *   **Cookie Consent:** The "UK Cookie Consent" plugin is used to manage user consent for cookies, likely for GDPR compliance.
    *   **Responsive Design:** The theme's CSS structure suggests a responsive design that adapts to different screen sizes (mobile, tablet, desktop).
    *   **SVG Support:** The "SVG Support" plugin is installed to allow for the use of scalable vector graphics, which are lightweight and high-quality.

## 3. Installation & Setup

This project appears to be a standard WordPress installation, likely managed with Composer.

### Prerequisites

*   PHP 7.4+
*   MySQL or MariaDB
*   A web server (e.g., Apache, Nginx)
*   Composer for PHP dependency management

### Installation Steps

1.  **Clone the Repository:**
    ```bash
    git clone <repository_url>
    ```
2.  **Install Dependencies:**
    Navigate to the `app/themes/galvanizers` directory and run:
    ```bash
    composer install
    ```
    This will install the Timber library and other PHP dependencies.
3.  **WordPress Configuration:**
    *   Set up a local WordPress installation.
    *   Configure the `wp-config.php` file with the correct database credentials and security salts.
    *   **Important:** Based on the plugins present, this is likely a **WordPress Multisite** installation. Ensure Multisite is enabled in `wp-config.php` by adding `define( 'WP_ALLOW_MULTISITE', true );` and following the network setup instructions in the WordPress admin dashboard.
4.  **Activate Plugins:**
    The following plugins are required and should be activated:
    *   `acf-content-analysis-for-yoast-seo`
    *   `advanced-custom-fields-pro` (not included, must be obtained separately)
    *   `better-search-replace`
    *   `contact-form-7`
    *   `debug-bar`
    *   `enhanced-media-library`
    *   `eps-301-redirects`
    *   `fakerpress`
    *   `google-analytics-for-wordpress`
    *   `health-check`
    *   `jetpack`
    *   `multisite-clone-duplicator`
    *   `redirection`
    *   `seo-by-rank-math`
    *   `svg-support`
    *   `temporary-login-without-password`
    *   `threewp-broadcast`
    *   `wordfence`
5.  **Activate Theme:**
    Activate the **Galvanizers** theme from the WordPress admin panel (`Appearance > Themes`).

## 4. Project Structure

The project follows a modern WordPress structure, separating the core application logic from the web root.

```
/
├── app/
│   ├── mu-plugins/       # Must-use plugins (e.g., Jetpack, Timber)
│   ├── plugins/          # Standard WordPress plugins
│   └── themes/
│       └── galvanizers/  # Main custom theme
│           ├── src/      # PHP source code (classes, etc.)
│           ├── templates/  # Twig templates for rendering
│           ├── assets/     # CSS, JS, images
│           └── functions.php # Theme setup
├── wp-content/           # Symlinked to app/
└── ... (other WordPress core files)
```

-   **`app/mu-plugins/`**: Contains critical plugins that are always active, such as Timber.
-   **`app/plugins/`**: Contains standard, installable WordPress plugins.
-   **`app/themes/galvanizers/`**: This is the main, custom-built theme for the project.
    -   **`src/`**: Houses the PHP classes that define custom post types, taxonomies, and other core theme logic.
    -   **`templates/`**: Contains the Twig files that define the HTML structure of the site, separating presentation from logic.
-   **`wp-content/`**: This directory is likely a symbolic link to the `app/` directory, a common practice in modern WordPress development for better organization and security.

## 5. Development & Maintenance

*   **Running Tests:** The `acf-content-analysis-for-yoast-seo` plugin includes a testing suite. Refer to its documentation for execution details.
*   **Styling:** Styles are written in SASS and compiled into CSS. The source files are located in `app/themes/twentytwentyone/assets/sass/`.
*   **Debugging:** The `debug-bar` plugin is installed, providing a powerful tool for debugging during development.

## 6. Limitations of this Analysis

This documentation was generated through automated analysis of the source code.
*   **Configuration:** Specific configurations stored in the database (like plugin settings, widget placements, menu structures) are not available.
*   **Content:** The actual content of posts, pages, and custom fields is unknown.
*   **Assumptions:** The analysis assumes this is a WordPress Multisite installation based on the plugins present. This should be verified.

<!-- From supporting phase -->
# WordPress Application Codebase

## Inferred Purpose and Domain

This repository contains the codebase for a sophisticated, multi-functional WordPress application. Based on the plugins and themes present, it appears to be a professionally managed, content-rich platform, likely operating as a WordPress Multisite network.

The project emphasizes:
- **Security:** With dedicated plugins like Wordfence and Malcare.
- **SEO and Content Quality:** Using Rank Math for SEO and integrations for analyzing content within Advanced Custom Fields (ACF).
- **Performance:** Leveraging a professional Redis object cache backend and the Breeze caching plugin.
- **Complex Content Management:** Extensive use of Advanced Custom Fields Pro and content syndication across sites with ThreeWP Broadcast.

The active custom theme is likely `galvanizers`, suggesting the business domain may be related to this name.

## Key Features & Capabilities

*   **Multisite Content Syndication:** The `threewp-broadcast` plugin allows for broadcasting posts and content across multiple sites in the network.
*   **Advanced Content Fields:** `acf-pro` enables the creation of complex and structured content types.
*   **High-Performance Caching:** Utilizes `redis-cache-pro` for a persistent object cache, significantly improving database performance at scale.
*   **Comprehensive Security:** A multi-layered security approach with Wordfence (firewall, scanner) and Malcare.
*   **Advanced SEO:** `seo-by-rank-math` provides in-depth search engine optimization tools, including Schema/structured data support.
*   **Modern Theme Architecture:** The custom theme `galvanizers` appears to use the Timber library, implementing an MVP (Model-View-Presenter) pattern with Twig templates for a clean separation of logic and presentation.

## Technology Stack

*   **Backend:** PHP (WordPress)
*   **Frontend:** JavaScript (jQuery), SCSS/CSS
*   **Database:** MySQL / MariaDB (Standard for WordPress)
*   **Caching:** Redis
*   **Package Management:**
    *   PHP: `Composer`
    *   JS: `npm`
*   **Build Tools:** `parcel-bundler` (used in some plugins)
*   **Development:** `FakerPress` for content generation and `Debug Bar` for debugging are present.

## Installation

**Prerequisites:**
*   A web server (e.g., Apache, Nginx)
*   PHP (Version 7.4+ recommended for modern WordPress)
*   MySQL or MariaDB
*   Redis server (for the `redis-cache-pro` plugin)
*   Composer (for PHP dependencies)
*   Node.js and npm (for frontend dependencies/build steps)

**Setup Instructions (Inferred):**

1.  **Clone the Repository:**
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Install PHP Dependencies:**
    Some plugins, like `threewp-broadcast`, use Composer. Run the following command in the plugin's directory or the project root if there is a root `composer.json`.
    ```bash
    composer install
    ```

3.  **Install JavaScript Dependencies:**
    Some plugins and themes use npm for managing frontend assets. Run `npm install` in directories containing a `package.json` file, such as `app/plugins/uk-cookie-consent/`.
    ```bash
    # Example for a specific plugin
    cd app/plugins/uk-cookie-consent
    npm install
    npm run build
    ```

4.  **WordPress Configuration:**
    *   This project likely uses a custom directory structure. The web server's document root should be configured to point to the directory containing WordPress's `index.php`.
    *   Create a `wp-config.php` file in the WordPress root directory. Configure database credentials, salts, and any other required constants.
    *   Given the presence of `redis-cache-pro`, the `wp-config.php` file will require specific constants to enable and configure the Redis object cache.

5.  **Database Setup:**
    *   Create a new database for the WordPress installation.
    *   Import the database from an existing dump if one is available. Otherwise, proceed with the standard WordPress installation by visiting the site URL in a browser.

6.  **Activate Plugins and Theme:**
    *   Log in to the WordPress admin dashboard.
    *   Ensure all required plugins from the `app/mu-plugins/` and `app/plugins/` directories are active.
    *   Activate the `galvanizers` theme.

## Usage Guide

*   **Accessing the Site:** Navigate to the configured site URL.
*   **Admin Area:** Access the WordPress dashboard at `/wp-admin/`.
*   **Key Functionality:**
    *   **Content Creation:** Use the standard WordPress editor. Advanced Custom Fields will be available on relevant post types.
    *   **Broadcasting:** When creating or editing a post, the ThreeWP Broadcast meta-box will be available to select target sites for duplication.
    *   **SEO:** The Rank Math meta-box will appear on post edit screens for SEO analysis and schema configuration.
    *   **Redirects:** Manage site redirects via the "Redirection" or "EPS 301 Redirects" plugin settings in the admin menu.

## Project Structure

The codebase appears to follow a modern WordPress structure where `wp-content` is likely renamed to `app` for better version control and organization.

```
.
├── app/
│   ├── mu-plugins/      # Must-use plugins (always active)
│   │   ├── acf-pro/
│   │   ├── jetpack/
│   │   ├── redis-cache-pro/
│   │   └── ...
│   ├── plugins/         # Standard plugins
│   │   ├── wordfence/
│   │   ├── seo-by-rank-math/
│   │   ├── threewp-broadcast/
│   │   └── ...
│   └── themes/          # Themes
│       ├── galvanizers/     # Inferred active custom theme
│       ├── twentytwentyfour/
│       └── ...
├── wp-content/          # May contain uploads or be partially used
│   └── ...
└── index.php            # WordPress main entry point
```

## Development Guidelines

*   **Dependency Management:** Use `composer` for PHP packages and `npm` for JavaScript packages as defined within individual plugin/theme directories.
*   **Testing:** The presence of `tests` directories in several plugins (`acf-content-analysis-for-yoast-seo`, `threewp-broadcast`) indicates that unit testing is part of the development workflow. Follow existing testing patterns when adding new functionality.
*   **Automated Updates:** A `renovate.json` file is present, suggesting that project dependencies are kept up-to-date automatically.

## Analysis Methodology and Limitations

*   This documentation was generated based on a static analysis of 220 files from the codebase. It does not represent the entire application (e.g., WordPress core files and the full code for every plugin were not available).
*   The active theme and plugins are inferred based on the files provided. The actual live configuration may differ.
*   Potential conflicts between plugins with overlapping functionality (e.g., caching, SEO) are noted but cannot be confirmed without access to the running application.
*   The analysis assumes this is a WordPress Multisite installation due to the presence of plugins specifically designed for that environment.

<!-- From documentation phase -->
# Galvanizers WordPress Website

## 1. Project Overview

This repository contains the codebase for the Galvanizers WordPress website. Based on the code structure and content, this is a sophisticated, content-driven platform, likely serving as a corporate presence with integrated "Magazine" and "Newsroom" sections.

The project is built on a modern WordPress stack that deviates from a standard installation, employing practices and tools designed to improve maintainability, performance, and developer experience. It appears to be a **WordPress Multisite** installation, designed to manage a network of related websites from a single WordPress instance.

The application is hosted on **Cloudways**, and its configuration is tightly coupled with that environment.

### Key Features

*   **Custom Theme (`galvanizers`):** A bespoke theme built with modern PHP (namespaces, OOP) and the Timber templating engine.
*   **MVC-like Architecture:** Utilizes the **Timber library** to separate business logic (PHP) from presentation (Twig templates), leading to cleaner and more maintainable code.
*   **Modern File Structure:** Employs a directory structure inspired by **Bedrock**, which organizes the codebase more logically than a standard WordPress setup.
*   **Multisite Ready:** Includes the "ThreeWP Broadcast" plugin, indicating it's designed to run as a WordPress Multisite network, allowing for content sharing between sites.
*   **Comprehensive Plugin Suite:** Integrates a wide array of plugins for SEO (`Rank Math`), security (`Wordfence`, `Malcare`), performance (`Breeze`), and advanced content management.
*   **Developer-Friendly:** Managed via **WP-CLI** and organized for modern development workflows.

## 2. Technology Stack

*   **CMS:** WordPress (Multisite)
*   **Backend:** PHP
*   **Frontend:**
    *   **Templating:** Twig (via the Timber library)
    *   **Styling:** SCSS / CSS
    *   **JavaScript:** jQuery, Vanilla JS
*   **Key Libraries & Frameworks:**
    *   **Timber:** For integrating Twig with WordPress.
    *   **Advanced Custom Fields (ACF):** Implied by the presence of an ACF Options Page class.
*   **Hosting:** Cloudways
*   **Security:** Wordfence, Malcare WAF
*   **Caching:** Breeze

## 3. Installation and Setup

**Disclaimer:** The following instructions are inferred from the project structure. Key configuration files like a root `composer.json` and `.env` were not provided in the analysis.

### Prerequisites

*   PHP
*   A web server (Nginx or Apache)
*   MySQL/MariaDB database
*   [Composer](https://getcomposer.org/)
*   [WP-CLI](https://wp-cli.org/)

### Setup Steps

1.  **Clone the Repository:**
    ```bash
    git clone <repository-url>
    cd <repository-name>
    ```

2.  **Install Dependencies:**
    The project structure strongly suggests Composer is used for managing dependencies.
    ```bash
    composer install
    ```
    *Note: This command assumes a `composer.json` file exists in the project root. If not, dependencies may need to be installed manually.*

3.  **Configure Environment:**
    This project likely uses a `.env` file for environment-specific configuration. Create a `.env` file in the project root by copying from a `.env.example` (if one exists).

    Fill in your local environment details:
    ```dotenv
    DB_NAME=your_db_name
    DB_USER=your_db_user
    DB_PASSWORD=your_db_password
    DB_HOST=localhost

    WP_ENV=development
    WP_HOME=http://your-local-domain.test
    WP_SITEURL=${WP_HOME}/wp

    # Generate salts using https://roots.io/salts.html
    AUTH_KEY='...'
    SECURE_AUTH_KEY='...'
    # ... and so on for all salts
    ```

4.  **Database Setup:**
    *   Create a new database for the project.
    *   Import the database from a production dump or install a fresh WordPress instance using WP-CLI.

5.  **Web Server Configuration:**
    Configure your web server (Nginx/Apache) to use `web/` as the document root. Ensure requests are properly routed to `web/index.php`.

## 4. Project Structure

The project uses a modern, Bedrock-inspired file structure that separates application code from the WordPress core. However, it also contains a traditional `wp-content` directory, suggesting a hybrid approach or a migration in progress.

```
.
├── app/                  # Application code (themes, plugins)
│   ├── mu-plugins/       # Must-Use plugins (Timber, Jetpack, etc.)
│   ├── plugins/          # Composer-managed or primary plugins
│   └── themes/
│       └── galvanizers/  # Main custom theme
│           └── src/      # PHP source code for the theme
├── code/                 # Additional custom code (e.g., menu definitions)
├── malcare-waf.php       # Malcare Web Application Firewall
├── web/                  # Web root
│   ├── app/              # Symlink to ../app
│   ├── wp/               # WordPress core installation
│   └── wp-content/       # Legacy or manually installed plugins/themes
└── wp-cli.yml            # WP-CLI configuration
```

### Key Files & Directories

*   `app/themes/galvanizers/`: The primary custom theme containing all unique frontend logic and templates.
*   `app/mu-plugins/`: Contains critical plugins like `timber-library` that are fundamental to the site's operation.
*   `code/Galvanizers/`: Contains PHP classes for registering menus and ACF options pages, likely part of the custom theme or a core functionality plugin.
*   `wp-content/`: Contains additional plugins and themes. The presence of `seo-by-rank-math` in both `app/plugins` and `wp-content/plugins` is a duplication that should be investigated.
*   `malcare-waf.php`: A security file auto-prepended by the Malcare plugin. It contains hardcoded paths specific to the Cloudways hosting environment.

## 5. Development Guidelines

### Theme Development

*   The `galvanizers` theme uses **Timber and Twig**.
*   **PHP logic** (Controllers) is handled in the theme's PHP files (e.g., `single.php`, `page.php`). These files fetch data and pass it to Twig templates.
*   **HTML markup** (Views) is located in `.twig` files within the theme's `views` directory (inferred standard practice for Timber).
*   Custom Post Types and Taxonomies are defined programmatically via the registerer classes in `app/themes/galvanizers/src/`.

### Running Tests

No dedicated testing suite (`PHPUnit`, etc.) was identified for the custom `galvanizers` theme. Some of the included plugins contain their own test suites.

### Contribution Process

A `CONTRIBUTING.md` file was not found. The contribution process is unclear from this analysis.

## 6. License

No `LICENSE` file was found in the repository. The licensing status is unknown.

## 7. Analysis Methodology and Limitations

*   This documentation was generated based on a static analysis of 134 files from the codebase.
*   **Assumption:** The project relies on Composer for PHP dependency management and an `.env` file for configuration, as is standard for this file structure. These files were not available for analysis.
*   **Limitation:** No build tool configuration (`package.json`, etc.) was provided, so the process for compiling frontend assets (SCSS to CSS) is unknown.
*   **Inconsistency:** The codebase contains duplicate plugin directories (`seo-by-rank-math`) and a mix of modern (`app/`) and traditional (`wp-content/`) structures. This may impact maintainability and should be reconciled.
