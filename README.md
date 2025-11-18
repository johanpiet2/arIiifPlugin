# arIiifPlugin - IIIF Integration for AtoM

A comprehensive IIIF (International Image Interoperability Framework) plugin for AtoM (Access to Memory) that provides deep zoom image viewing capabilities using OpenSeadragon and Cantaloupe image server integration.

---

## 📛 Status & Compatibility

<p align="left">
  <img src="https://img.shields.io/badge/AtoM-2.9.x-blue" />
  <img src="https://img.shields.io/badge/PHP-8.3+-green" />
  <img src="https://img.shields.io/badge/Platform-Ubuntu%2024.04-orange" />
  <img src="https://img.shields.io/badge/License-AGPLv3-red" />
  <img src="https://img.shields.io/badge/IIIF-Image-Viewer-purple" />
  <img src="https://img.shields.io/badge/Maintainer-Johan%20Pieterse-lightgrey" />
</p>

---

## Features

- **IIIF Image Viewer**: OpenSeadragon-based viewer with deep zoom capabilities
- **Image Carousel**: Multiple image support with auto-rotation
- **IIIF Manifest Generation**: Presentation API 2.1 compliant manifests
- **Cantaloupe Integration**: Seamless integration with Cantaloupe IIIF image server
- **Responsive Design**: Mobile-friendly viewing experience
- **Thumbnail Navigation**: Quick image browsing with thumbnail strip
- **Download Support**: Direct image download functionality

## Requirements

- AtoM 2.6+ (tested with 2.9)
- PHP 7.2+ (tested with PHP 8.3)
- Cantaloupe 5.0.6+ IIIF Image Server
- Web server with HTTPS support
- Modern web browser with JavaScript enabled

## Installation

### Quick Install
```bash
# Navigate to your AtoM plugins directory
cd /usr/share/nginx/atom/plugins

# Clone the repository
git clone https://github.com/yourusername/arIiifPlugin.git

# Run the installation script
cd arIiifPlugin
sudo bash install.sh
```

### Manual Installation

See [INSTALL.md](INSTALL.md) for detailed installation instructions.

## Configuration

### 1. Cantaloupe Setup

Ensure your Cantaloupe server is properly configured with:
- `slash_substitute = _SL_` in cantaloupe.properties
- Proper delegates.rb for file path resolution
- Maximum pixel limits adjusted for your images

### 2. Plugin Configuration

Edit `/usr/share/nginx/atom/plugins/arIiifPlugin/config/app.yml`:
```yaml
all:
  iiif:
    base_url: https://yourdomain.com/iiif/2
    api_version: 2
    server_type: cantaloupe
    enable_manifests: true
    
    carousel:
      auto_rotate: true
      rotate_interval: 5000
      show_navigation: true
      show_thumbnails: false
      viewer_height: 600
```

### 3. AtoM Configuration

Add to your AtoM's `app.yml`:
```yaml
all:
  iiif_base_url: https://yourdomain.com/iiif/2
```

### 4. Enable the Plugin

Add to your AtoM configuration file (`config/ProjectConfiguration.class.php`):

```php
public function setup()
{
  $this->enablePlugins(array(
    // ... other plugins
    'arIiifPlugin'
  ));
}
```

## Usage

### Basic Viewer

To display a single IIIF image:
```php
<?php include_component('arIiifPlugin', 'viewer', array(
  'resource' => $digitalObject
)) ?>
```

### Image Carousel

For multiple images with carousel:
```php
<?php include_component('arIiifPlugin', 'carousel', array(
  'resource' => $informationObject,
  'autoRotate' => true,
  'showThumbnails' => true
)) ?>
```

### IIIF Manifest URLs

The plugin automatically generates IIIF manifests at:
- Information Object: `/iiif/{slug}/manifest`
- Digital Object: `/iiif/object/{id}/manifest`

## Troubleshooting

### Images not loading

1. Check Cantaloupe is running: `sudo systemctl status cantaloupe`
2. Verify file permissions: Files should be readable by www-data
3. Check browser console for JavaScript errors
4. Verify IIIF URLs are accessible

### 403 Forbidden errors

- Increase `max_pixels` in cantaloupe.properties
- Check file permissions in uploads directory

### Viewer shows black screen

- Ensure OpenSeadragon library is loaded
- Check IIIF info.json response
- Verify CORS headers are set correctly

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This plugin is licensed under the GNU General Public License v3.0

## Credits

- Developed for The AHG archives (theahg.co.za)
- Built on [OpenSeadragon](https://openseadragon.github.io/)
- Integrates with [Cantaloupe](https://cantaloupe-project.github.io/)
- For use with [AtoM](https://www.accesstomemory.org/)

## Support

For issues and questions:
- Create an issue on GitHub
- Contact: johan@theahg.co.za
- AtoM Forum: https://groups.google.com/forum/#!forum/ica-atom-users

## Version History

- **1.0.0** (2025-11-14): Initial release
