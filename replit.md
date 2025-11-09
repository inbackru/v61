# Overview

InBack/Clickback is a Flask-based real estate platform specializing in cashback services for new construction properties in the Krasnodar region. It connects buyers and developers, offers property listings, streamlines application processes, and integrates CRM tools. The platform aims to provide unique cashback incentives, an intuitive user experience, intelligent property search with interactive maps, residential complex comparisons, user favorites, a manager dashboard for client and cashback tracking, and robust notification and document generation capabilities. The project is currently expanding to support multiple cities across Krasnodarsky Krai and the Republic of Adygea.

# Recent Changes (2025-11-09)

## Dashboard Final Polish & Brand Consistency (Latest)
- **Header Navigation Enhancement**: Added "На главную" button as first nav item in dashboard header bar (before "Главная" tab) for quick homepage access. Uses standard href="/" without data-page attribute to avoid tab-switching conflicts
- **Sidebar Favorites Counter Fixed**: Corrected sidebar badge to show combined total of properties + complexes (e.g., "9" for 5 properties + 4 complexes). Backend imports FavoriteComplex model and sums both counts. Browser logs confirm: "🏠 Обновлен счетчик избранного в меню: 9 (5 квартир + 4 ЖК)"
- **Brand Color Consistency**: Changed saved-searches stat cards and loading spinners from purple gradients to brand colors (`from-[#0088CC] to-[#006699]`) matching company identity
- **Optimized Button Sizing**: Reduced "Применить" button in saved-searches to px-4 py-2 (from px-5 py-2.5) and shortened label from "Применить поиск" to "Применить" for cleaner, more compact appearance
- **Badge "новое" Persistence Verified**: Confirmed viewFavoriteProperty() function correctly calls `/api/favorites/mark-viewed` API before redirect, updating viewed flag to remove "новое" badge on subsequent dashboard loads

## Dashboard Cleanup & Navigation Fixes (2025-11-09)
- **Removed Duplicate Favorites Section**: Eliminated 335-line duplicate "Favorites" section containing obsolete cashback calculator code from `templates/auth/dashboard.html`. Now only one clean `id="favorites"` section remains with modern stats cards and property/complex lists
- **Fixed Duplicate Element IDs**: Resolved HTML conflicts caused by duplicate `id="applications"` and `id="favorites"` sections, preventing JavaScript and rendering issues
- **Streamlined Sidebar Navigation**: Removed redundant "На главную" button from sidebar_links in app.py since the sidebar logo already links to homepage. Set "Главная" (#dashboard) tab as active by default for cleaner navigation structure
- **Manager Avatar Support**: Enhanced recommendations display to correctly render HTTP/HTTPS URLs, relative paths starting with `/`, and paths containing "uploads/" or "static/"
- **Status Badge Visibility**: Improved status badges with larger size (px-4 py-1.5), animated white pulsing dot, and stronger gradient backgrounds for better visibility
- **Corrected Recommendations Counter**: Fixed sidebar badge to count Collection records (presentations) filtered by assigned_to_user_id instead of Recommendation records

## Dashboard Modernization - Complete (2025-11-08)
- **Saved Searches Tab**: Modernized with gradient stat cards enhanced loading states with animate-ping effects, and modern card design with rounded-2xl borders and hover:scale-105 transitions
- **Recommendations Tab**: Updated with three gradient stat cards (blue-to-indigo for new presentations, emerald-to-teal for total, orange-to-amber for manager info), consistent white/20 opacity icon backgrounds, and professional loading animations
- **Comparison Tab**: Fully redesigned with gradient stat cards (indigo-to-purple for properties, blue-to-cyan for complexes, violet-to-fuchsia for detailed comparison), enhanced buttons with gradient backgrounds, and unified loading states matching 21st.dev design patterns
- **Design Consistency**: All dashboard tabs now feature consistent gradient backgrounds, rounded-2xl borders, white/20 opacity overlays for icons, hover:scale-105 transitions, and modern loading states with dual-layer animations (ping effect + spinning icon)

## Previous Features & Improvements
- **Dashboard Sidebar Integration**: Fully integrated collapsible sidebar into both user dashboard (`templates/auth/dashboard.html`) and manager dashboard (`templates/manager/dashboard.html`). Features dynamic navigation links with real-time badge counters (favorites, cashback applications, clients, etc.), user profile footer with role indicator, and responsive layout with `md:ml-[70px]` spacing. Backend routes updated to pass `sidebar_links` and `user_profile` context data.
- **Collapsible Sidebar Component**: Professional Aceternity-style sidebar for dashboard navigation with smooth animations, hover-to-expand behavior, mobile-responsive design, and minimalist aesthetics. Features include auto-collapse on desktop (expands on hover), mobile hamburger menu, user profile section, and badge support for notifications. Located in `templates/components/sidebar.html`.
- **City Selector Modal**: Professional Yandex.Nedvizhimost-style modal for city selection with live search, quick-select buttons (8 cities), auto-detect option, and localStorage persistence. Integrated in both desktop header and mobile menu.
- **Property Appraisal Page**: Minimalist `/appraisal` (and `/otsenka`) page for property evaluation services, featuring hero section with form, 4-card "When to Contact" section, service description, CTA section, and benefits grid. Replaces "Подбор банка" in header navigation under Mortgage mega-menu.
- **Street Pages Bug Fix**: Fixed SQL syntax error in `street_detail` route where Python code was incorrectly embedded inside SQL query, causing 404 errors on street pages like `/street/1-maya`.

## SEO & Security Improvements (2025-11-06)
- **HSTS Headers**: Added `Strict-Transport-Security` header (max-age=31536000) for enhanced security and SEO
- **Enhanced Organization Schema**: Expanded JSON-LD schema on homepage with ratings, hours, service catalog
- **FAQ Schema**: Added FAQPage structured data to ipoteka.html with 6 key questions for rich snippets
- **Yandex.Metrika**: Analytics already configured (ID: 104270300) with webvisor, clickmap, ecommerce tracking
- **Cashback Terms Page**: Created comprehensive `/cashback-terms` (English) and `/usloviya-keshbeka` (Russian) pages with legal details, FAQ, payment timeline, and full transparency. Both URLs serve the same content for SEO and user convenience.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend

The frontend uses server-side rendered HTML with Jinja2 and CDN-based TailwindCSS for a mobile-first, responsive design. Interactivity is managed by modular vanilla JavaScript, including smart search, real-time filtering, Yandex Maps integration, property comparison, and PDF generation.

## Backend

Built with Flask 2.3.3, the backend employs an MVC pattern with blueprints. SQLAlchemy with PostgreSQL serves as the ORM. Key features include Flask-Login for session management and RBAC (Regular Users, Managers, Admins), robust security, and custom parsing for Russian address formats. The system supports phone verification and manager-to-client presentation delivery. Data is structured using normalized Developers → Residential Complexes → Properties schemas and a Repository Pattern for data access. The system supports multi-city data management, dynamic city selection, and city-aware data filtering.

## Data Storage

PostgreSQL, managed via SQLAlchemy 2.0.32, is the primary database. The schema includes Users, Managers, Properties, Residential Complexes, Developers, Marketing Materials, transactional records, and search analytics. Flask-Caching optimizes performance. `residential_complexes` includes rich content fields and a `complex_type` field. `marketing_materials` stores promotional content linked to residential complexes.

## Authentication & Authorization

The system supports Regular Users, Managers, and Admins through a unified Flask-Login system with dynamic user model loading.

## Intelligent Address Parsing & Search System

This system leverages DaData.ru for address normalization and Yandex Maps Geocoder API for geocoding. It features auto-enrichment for new properties, optimized batch processing, smart caching, and city-aware address suggestions. The database schema supports granular search and filtering.

## UI/UX and Feature Specifications

-   **AJAX-powered Sorting and Filtering:** Property listings feature an AJAX API for fast, scalable sorting and filtering, with infinite scroll and view mode persistence.
-   **Residential Complex Image Sliders:** Multi-photo sliders with fallback.
-   **Comparison System:** PostgreSQL-backed comparison for properties and complexes with strict limits.
-   **Interactive Map Pages:** Full-height Leaflet and Yandex Maps display residential complexes and properties with color-coded markers, grouping, server-side filtering, sticky search/filter bars, and mobile-optimized bottom sheets.
-   **Unified Filter + Search Row (Desktop):** Consistent desktop design for `/residential-complexes` and `/properties` pages, combining filters and search in a single horizontal row with SuperSearch suggestions and filter badge counters.
-   **Mobile Sticky Search Bar:** Avito-style compact sticky search bar with backdrop blur, bidirectional search input sync, and integrated controls. Features include fullscreen filter overlays with real-time object counters and advanced filters.
-   **Fullscreen Map Modals (Mobile):** Interactive Yandex Maps modals for complexes and properties, with compact filter toolbars, bottom sheet quick filters, and advanced filter modals.
-   **Saved Search Feature:** Fullscreen mobile modal for saving property searches, with authentication checks, filter previews, and smart search name generation.
-   **Smart Search with Database-Backed History:** Real-time suggestions via SuperSearch, supporting flexible query formats and tracking for analytics.
-   **Dynamic Results Counter:** Updates property count with correct Russian declension.
-   **Property Alert Notification System:** Comprehensive system for saved searches with instant, daily, and weekly alerts via email and Telegram, configurable frequency, delivery channels, and one-click unsubscribe.
-   **Marketing Materials Management:** Comprehensive management system for residential complex marketing materials, including database storage, admin panel CRUD, quick-add functionality, and file validation.
-   **Action Buttons on Detail Pages:** User-facing "Favorite," "Compare," and "Share" buttons on property and residential complex detail pages.
-   **Excursion/Online Showing CTA Blocks:** Conversion-focused call-to-action blocks on complex and property detail pages respectively, integrating with application modals.
-   **City Selector UI:** Dynamic city selection and persistence across the platform with correct Russian grammar declensions.
-   **Automatic City Detection:** Detects user's city via IP and sets it in the session for localized content.

## Comprehensive SEO Optimization

The platform implements production-ready multi-city SEO optimization covering all 8 cities (Краснодар, Сочи, Анапа, Геленджик, Новороссийск, Армавир, Туапсе, Майкоп):

-   **Canonical URLs:** All pages use HTTPS production domain (https://inback.ru) to prevent duplicate content issues and consolidate SEO authority.
-   **City-Aware Meta Tags:** Dynamic titles and descriptions interpolate city names with proper Russian grammatical cases (genitive for "Краснодара", prepositional for "в Краснодаре").
-   **JSON-LD Structured Data:** All property and complex pages include schema.org markup with `addressLocality` and `addressRegion` fields for rich search results.
-   **Regional Variations:** Correctly handles Maykop in "Республика Адыгея" versus 7 other cities in "Краснодарский край" for accurate regional SEO.
-   **Comprehensive Sitemap:** Auto-generated sitemap.xml with 384 URLs covering homepage, city-specific properties/complexes pages, individual property details, mortgage pages, and developer pages across all cities.
-   **SEO-Friendly URLs:** City-based routing using city slugs (`/sochi/properties`, `/maykop/residential-complexes`, `/krasnodar/object/123`) for better indexing and user experience.
-   **robots.txt Configuration:** Properly configured to guide search engine crawlers and reference sitemap location.

# External Dependencies

## Third-Party APIs

-   **SendGrid**: Email sending.
-   **OpenAI**: Smart search and content generation.
-   **Telegram Bot API**: User notifications and communication.
-   **Yandex Maps API**: Interactive maps, geocoding, and location visualization.
-   **DaData.ru**: Address normalization, suggestions, and geocoding.
-   **SMS.ru, SMSC.ru**: Russian SMS services for phone verification.
-   **Google Analytics**: User behavior tracking.
-   **LaunchDarkly**: Feature flagging.
-   **Chaport**: Chat widget.
-   **reCAPTCHA**: Spam and bot prevention.
-   **ipwho.is**: IP-based city detection.

## Web Scraping Infrastructure

-   `selenium`, `playwright`, `beautifulsoup4`, `undetected-chromedriver`: Used for automated data collection.

## PDF Generation

-   `weasyprint`, `reportlab`: Used for generating property detail sheets, comparison reports, and cashback calculations.

## Image Processing

-   `Pillow`: Used for image resizing, compression, WebP conversion, and QR code generation.