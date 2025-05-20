# Customizing Zabbix Branding and Icons in Nginx

Since the beginning of the russian invasion of Ukraine, the simple letter "Z" has become a symbol of pain, blood, torture, and a new form of Nazism, disguised under the pretense of "liberation" of a peaceful people and a peaceful country from something unknown — perhaps from life itself, or at least from peace. This guide has been created precisely for this reason: although the developers of Zabbix are not primarily russians, some among them likely promote their unwelcome perspectives. I don't want people to see my screen somewhere in public and think I am a ru-nazi supporter, so I prefer to change the icon.

Follow the steps below to replace Zabbix's default icons and branding elements. This method utilizes Nginx configuration and custom icon files. This manual assumes the person has some experience with Linux and Nginx configuration.

This method worked on version 5.4 and is still effective on Zabbix version 7.2 (but the branding path changed, see notice).

## Notice!

**For Zabbix versions before 7.2, the branding path begins with `/usr/share/zabbix/`, so the branding php file must be located at `/usr/share/zabbix/local/conf/brand.conf.php` and images should be located in the subdirectory of `/usr/share/zabbix/` (for example, `/usr/share/zabbix/branding/`).**

**Starting with version 7.2, the branding paths start from `/usr/share/zabbix/ui/`, so the branding php file must be located at `/usr/share/zabbix/ui/local/conf/brand.conf.php` and images should be located in the subdirectory of `/usr/share/zabbix/ui/` (for example, `/usr/share/zabbix/ui/branding/`). Also, it means the root directory of the UI is `/usr/share/zabbix/ui/` in the web server configuration.**

There is another [instruction](https://git.initmax.cz/initMAX-Public/Zabbix-UI-Rebranding-skeleton), and someone may find it more friendly. They posted it a few days after my instructions.

## Nginx Configuration

In the Nginx configuration for Zabbix, under the relevant `server` section, add the following lines to handle the custom icons:

```nginx
location = /favicon.ico {
    log_not_found   off;
    alias /usr/share/zabbix/ui/branding/custom-icons/favicon.ico;
    expires max;
}
location ~* /assets/img/apple-touch-icon-(76x76|120x120|152x152|180x180)-precomposed\.png {
    alias /usr/share/zabbix/ui/branding/custom-icons/apple-touch-icon-$1-precomposed.png;
    expires max;
}
location ~* /assets/img/touch-icon-192x192\.png {
    alias /usr/share/zabbix/ui/branding/custom-icons/touch-icon-192x192.png;
    expires max;
}
```
Usually, the configuration file is in `/etc/nginx/sites-enabled/` or `/etc/nginx/conf.d/`. In Ubuntu, it is a link `/etc/nginx/conf.d/zabbix.conf -> /etc/zabbix/nginx.conf`.
After changing the config file, run `sudo systemctl reload nginx.service` to reload the configuration into Nginx.

Note: If the `favicon.ico` is already referenced elsewhere in the Nginx configuration, update that existing line rather than adding a new one, as multiple references to the same path may break the configuration.

## Creating Icons

You can generate the necessary icon files using ImageMagick or GIMP. These custom icons should be stored in the `/usr/share/zabbix/branding/custom-icons/` directory.

- favicon.ico
- apple-touch-icon-76x76-precomposed.png
- apple-touch-icon-120x120-precomposed.png
- apple-touch-icon-152x152-precomposed.png
- apple-touch-icon-180x180-precomposed.png
- touch-icon-192x192.png


## Custom Branding

To further customize Zabbix's appearance, you can modify the branding using a hidden feature that is not mentioned in the official manuals anymore. This can be done by creating or editing the following file:

**Path:** `/usr/share/zabbix/ui/local/conf/brand.conf.php`

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

**Note:** The branding feature is hidden for some reason, and the official documentation no longer mentions it. Some links that previously led to details about this feature now return a 404 error.
