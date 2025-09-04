=== Custom Logout ===   
Contributors: yehoon17  
Tags: logout, shortcode, security, redirect, link, button  
Requires at least: 5.0  
Tested up to: 6.6  
Requires PHP: 7.4  
Stable tag: 1.0.0  
License: GPLv2  
License URI: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html  

A tiny, extensible shortcode for secure logout links or buttons with redirect support.

== Description ==

**Custom Logout** provides a simple shortcode to render a secure logout **link** (default) or **form + button**. It uses WordPress’ built-in `wp_logout_url()` (nonce-protected) and supports a redirect target.

**Shortcode**: `[custom_logout]`

**Attributes:**
- `class` – Add CSS classes to the link/button. Example: `btn btn-secondary`
- `label` – Text for the link/button. Default: `Logout`
- `redirect` – URL to redirect to after logout. Default: site homepage.
- `type` – Output type: `link` (default) or `form` (renders `<form><button>`)

The plugin is **translation-ready** (Text Domain: `custom-logout`) and **extensible** via filters.

== Installation ==

1. Upload the `custom-logout` folder to `/wp-content/plugins/`, or install via the Plugins screen.
2. Activate **Custom Logout** through the Plugins screen.
3. Use `[custom_logout]` in any post, page, or widget area that supports shortcodes.

== Usage ==

**Basic (default link):**
```
\[custom\_logout]
```

**Button via form:**
```
\[custom\_logout type="form" class="btn btn-primary" label="Sign out"]
```

**Redirect somewhere specific:**
```
\[custom\_logout redirect="[https://example.com/goodbye](https://example.com/goodbye)"]
```

**Combine attributes:**
```
\[custom\_logout type="form" class="btn btn-secondary w-100" label="Log me out" redirect="/after-logout"]
````

== Security Notes ==

- The **link** output uses `wp_logout_url( $redirect )`, which includes a secure nonce to protect against CSRF.
- The **form** output posts to `wp-login.php` with `action=logout`, a valid nonce, and `redirect_to`. WordPress validates the nonce (`check_admin_referer('log-out')`) and performs the redirect.

== Filters ==

Developers can customize output using these filters:

1. **`custom_logout_output`**

   Filter the final rendered HTML. Useful for wrapping, adding icons, changing attributes, etc.

   **Signature:**
   ```php
   apply_filters( 'custom_logout_output', $html, $context );
   ```

    **Parameters:**

    * `$html` (string) Final HTML.
    * `$context` (array) Includes:
      * `atts`       (array) Raw shortcode atts after `shortcode_atts`.
      * `type`       (string) 'link' or 'form'.
      * `class`      (string) Sanitized classes.
      * `label`      (string) Visible text for link/button.
      * `redirect`   (string) Final validated redirect URL.
      * `logout_url` (string) Nonce-protected logout URL (used for link).
      * `action_url` (string) Login endpoint URL (form post target).

    **Example:**

    ```php
    add_filter( 'custom_logout_output', function( $html, $ctx ) {
        // Add a wrapper div.
        return '<div class="my-logout-wrap">' . $html . '</div>';
    }, 10, 2 );
    ```

2. **`custom_logout_logged_out_output`**

   Filter the output when the **user is already logged out**. Default: empty string.

   **Signature:**

   ```php
   apply_filters( 'custom_logout_logged_out_output', $html, $context );
   ```

   **Parameters:**

   * `$html`    (string) Default empty string.
   * `$context` (array) Includes:
     * `atts`     (array) Raw shortcode atts.
     * `redirect` (string) Final validated redirect URL.

   **Example:**

   ```php
   add_filter( 'custom_logout_logged_out_output', function( $html, $ctx ) {
       return '<a href="' . esc_url( wp_login_url() ) . '" class="login-link">🔑 ' . esc_html__( 'Log in', 'custom-logout' ) . '</a>';
   }, 10, 2 );
   ```

\== Changelog ==

\= 1.0.0 =

* Initial release: shortcode, filters, translation-ready, GPLv2.

\== Upgrade Notice ==

\= 1.0.0 =
Initial release.
