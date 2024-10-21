# Customizing Zabbix Branding and Icons in Nginx

To replace Zabbix's default icons and branding elements, follow the steps outlined below. This method utilizes Nginx configuration and custom icon files.

## Nginx Configuration

In the Nginx configuration for Zabbix, under the relevant `server` section, add the following lines to handle the custom icons:

```conf
location = /favicon.ico {
    log_not_found   off;
    alias /usr/share/zabbix/branding/custom-icons/favicon.ico;
    expires max;
}
location ~* /assets/img/apple-touch-icon-(76x76|120x120|152x152|180x180)-precomposed\.png {
    alias /usr/share/zabbix/branding/custom-icons/apple-touch-icon-$1-precomposed.png;
    expires max;
}
location ~* /assets/img/touch-icon-192x192\.png {
    alias /usr/share/zabbix/branding/custom-icons/touch-icon-192x192.png;
    expires max;
}
```

Note: If the `favicon.ico` is already referenced elsewhere in the Nginx configuration, update that existing line rather than adding a new one.

## Creating Icons

You can generate the necessary icon files using ImageMagick or GIMP. These custom icons should be stored in the `/usr/share/zabbix/branding/custom-icons/` directory.

With these changes, the only elements left that indicate this is a Zabbix system are the URL, overall design, and mentions in templates.

## Custom Branding

To further customize Zabbix's appearance, you can modify the branding using a hidden feature that is not mentioned in the official manuals anymore. This can be done by creating or editing the following file:

**Path:** `/usr/share/zabbix/local/conf/brand.conf.php`

### Full File Example:

```php
<?php
return [
    'BRAND_LOGO' => '/branding/custom-logo.png',
    'BRAND_LOGO_SIDEBAR' => '/branding/custom-sidebar-logo.png',
    'BRAND_LOGO_SIDEBAR_COMPACT' => '/branding/custom-sidebar-logo-compact.png',
    'BRAND_FOOTER' => 'CompanyName - Monitoring '.ZABBIX_VERSION
];
```

### Logo Sizes:

- Main logo: 114x30
- Sidebar logo: 91x24
- Compact sidebar logo: 24x24

**Important:** The branding feature is somewhat hidden, and the official documentation does not mention it anymore. Some links that previously led to details about this feature now return a 404 error.
