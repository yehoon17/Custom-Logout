# Custom Logout

[![License: GPL v2](https://img.shields.io/badge/License-GPLv2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)
A tiny, extensible WordPress plugin that provides a secure logout **shortcode** (`[custom_logout]`) with redirect and form options.


## Features

* Shortcode `[custom_logout]`
* Outputs a **logout link** (default) or a **form + button**
* Attributes:
  * `class` → CSS classes
  * `label` → Link text (default: "Logout")
  * `redirect` → Redirect URL after logout (default: homepage)
  * `type` → `link` (default) or `form`
* Secure logout via `wp_logout_url()` with nonce
* Translation-ready (`Text Domain: custom-logout`)
* Developer filters:
  * `custom_logout_output`
  * `custom_logout_logged_out_output`

## Installation

1. Download or clone this repo into your WordPress plugins folder:

   ```bash
   git clone https://github.com/yehoon17/custom-logout.git wp-content/plugins/custom-logout
   ```

2. Activate **Custom Logout** in your WordPress Admin under **Plugins → Installed Plugins**.

3. Add the shortcode `[custom_logout]` in pages, posts, or widgets.

## Usage

**Default (link):**

```html
[custom_logout]
```

**Button (form):**

```html
[custom_logout type="form" class="btn btn-primary" label="Sign out"]
```

**Redirect after logout:**

```html
[custom_logout redirect="https://example.com/goodbye"]
```

**Combined:**

```html
[custom_logout type="form" class="btn btn-secondary w-100" label="Log me out" redirect="/after-logout"]
```

## Developer Hooks

### `custom_logout_output`

Modify the final HTML output.

```php
add_filter( 'custom_logout_output', function( $html, $ctx ) {
    return '<div class="logout-wrap">' . $html . '</div>';
}, 10, 2 );
```

### `custom_logout_logged_out_output`

Define what shows when the user is **already logged out**.

```php
add_filter( 'custom_logout_logged_out_output', function( $html, $ctx ) {
    return '<a href="' . esc_url( wp_login_url() ) . '" class="login-link">Log in</a>';
}, 10, 2 );
```

## Internationalization

* Text domain: `custom-logout`
* Ready for translation (`.pot` file can be generated)

## License

This project is licensed under the [GPLv2 License](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html).

## Roadmap

* [ ] Add block editor support (Gutenberg block)
* [ ] Settings page for global defaults
* [ ] More extensibility hooks

## Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss your ideas.
