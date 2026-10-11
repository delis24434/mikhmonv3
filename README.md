Initialize Phase 1 for application "Archmon": Core Foundation, Dual-Path Data Adapter, and Standalone Futuristic Login Interface.

Preserve all existing baseline structures and adhere strictly to the following modular specifications. Do not generate placeholder pages, dashboards, navigation sidebars, header bars, or helper assistance text (such as "default user", demo notes, autofill links, or onboarding tips). The viewport must feature only the isolated login interface and its core system tools.

Wrap every created component and script inside explicit comment blocks marking its start and end with its semantic arc- identifier.

1. arc-version
- Define initial system version as V.0.0.0.
- Create an internal append-only registry structure for tracking modular changes and future upgrades.

2. arc-theme-config
- Establish two distinct, remembered aesthetic palettes: True Dark AMOLED (default, deep pure black canvas with subtle ambient fantasy-tech glow accents) and Day (eye-friendly neutral light surface with crisp cybernetic accents).
- Define dynamic semantic tokens ensuring maximum contrast between input text, container backgrounds, and vibrant RGB edge highlights. All element styles must read strictly from these tokens.

3. arc-ui-assets
- Build a zero-dependency, inline SVG icon registry designed for offline execution.
- Include ultra-compact, scalable vectors for: App Logo (cyber-fantasy glyph), Lock, User, Eye/Visibility, Theme Switch, Language Switch, Database Connected (Live), Database Disconnected/Mock, Close, Loading Spinner, Checkmark, and Alert Warning.

4. arc-i18n
- Build a centralized bilingual dictionary supporting Bahasa Indonesia (default) and English, with local persistence.
- Provide a reactive localization resolver. All visible user interface labels, validation states, tooltips, and toast messages must resolve exclusively through this dictionary without any hardcoded strings in layout templates.

5. arc-tooltips
- Implement a viewport-aware tooltip provider bound to all icon-only interactive controls.
- On pointer hover: Display a micro informational card positioned safely within screen boundaries.
- On touch devices: A quick tap executes the underlying primary action; a continuous press (~400ms) reveals the tooltip overlay without firing the action.

6. arc-toast-system
- Construct a high-priority notification system rendered strictly in the absolute vertical and horizontal center of the active viewport.
- Support modal notification variants: Information, Warning, Error, and Success.
- Include automatic dismiss timers, dynamic progress indicators, and manual close triggers, maintaining crisp typography and high-contrast atmospheric backdrops.

7. arc-smart-button
- Construct a reusable, state-aware button component displaying an icon-only surface backed by arc-tooltips.
- Incorporate distinct internal states: Idle, Loading with progress animation, Error, and Success.
- Provide an emergency abort/cancel capability during active asynchronous loading states.

8. arc-data-adapter
- Construct a resilient dual-mode transport layer managing database requests and session storage.
- Default to live operational mode. On initialization, invoke connection check via POST to "./api/arc-login-api.php".
- If the endpoint responds successfully: set data state to LIVE DB, reflect status via the interface indicator icon, and store connection state.
- If the endpoint is unreachable, network fails, or CORS/database errors arise:
  * Check entered credentials: if the username is "@Novafelix" and password is "Novafelix", cleanly engage Mock Mode.
  * Save authenticated Master Admin identity ({id: 1, username: "@Novafelix", full_name: "Arya Novafelix", role: "Admin", status: "Active"}) strictly into client localStorage.
  * Trigger a centered toast reporting the precise live database failure while confirming authenticated Mock authorization.
- Expose a manual ping action to re-evaluate database connectivity on demand, transitioning between Live and Mock states with immediate toast feedback.

9. api/arc-login-api.php
- Provide a standalone, secure backend script strictly compatible with PHP 7.2 (avoid typed properties, arrow functions, and features requiring PHP 7.4+).
- Begin with error suppression (error_reporting(0)) and enforce JSON output headers.
- Establish PDO database connection targeting:
  * Host: localhost
  * Database: arya24434_ajip
  * Username: arya24434_ajip
  * Password: @123456zZ
- Auto-Migration on initialization: Execute table creation if not exists for "arc_users" containing columns: id (INT AUTO_INCREMENT PRIMARY KEY), username (VARCHAR(50) UNIQUE NOT NULL), password (VARCHAR(255) NOT NULL), full_name (VARCHAR(100) NOT NULL), phone (VARCHAR(25) NULL), role (ENUM('Admin', 'Teknisi', 'Pelanggan', 'Reseller') DEFAULT 'Pelanggan'), status (ENUM('Active', 'Suspended') DEFAULT 'Active'), saldo_komisi (DECIMAL(15,2) DEFAULT 0.00), and created_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP).
- STRICT RULE: Do not insert test rows or dummy administrative data into the database table.
- Implement JSON action handlers:
  1. action "ping": Validates PDO connection, returning operational status or exact PDO error message.
  2. action "login": Validates incoming username and hashed password via native password_verify, returning sanitized user profile and session token.

10. arc-login-page
- Render a standalone, centered login viewport for "Archmon" utilizing a fantasy-tech/cyberpunk aesthetic without headerbars or sidebars.
- Layout elements:
  * Top accessory cluster: Live/Mock database status icon-only indicator, Language switcher icon-only toggle, and AMOLED/Day theme switcher icon-only toggle.
  * Central surface: Archmon insignia, application title, username input field, password input field with reveal toggle, and a primary arc-smart-button action trigger.
- Behavior:
  * Form submission initiates authentication through arc-data-adapter.
  * Successful authentication or validation errors must only trigger the centered arc-toast-system with descriptive state feedback.
  * Do not redirect or render secondary views, dashboards, or extra modules at this stage.
