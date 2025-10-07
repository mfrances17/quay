# Agent Webpack Development

## Overview

Comprehensive guide for Webpack development and optimization in Quay frontend.

## For Agents

### Processing Priority

Medium - This document should be processed when working with build configuration or optimization.

### Related Guidelines

See the [Guidelines Index](./README.md#guidelines-index) for a complete list of all guidelines.

### Key Concepts

- Webpack 5 configuration
- Build optimization
- Development vs production builds
- Asset management
- Performance optimization

## Webpack Architecture

### Current Configuration
- **Webpack**: 5.95.0 with modern features
- **Configuration Files**:
  - `webpack.dev.js`: Development configuration
  - `webpack.prod.js`: Production configuration
  - `webpack.plugin.js`: Plugin-specific configuration
- **Loaders**: TypeScript, CSS, Sass, file loaders
- **Plugins**: HTML, CSS extraction, optimization

### Build Scripts
```json
{
  "start": "webpack serve --color --progress --config webpack.dev.js",
  "build": "webpack --config webpack.prod.js",
  "start-plugin": "NODE_ENV=development webpack serve --color --progress --config webpack.plugin.js",
  "build-plugin": "NODE_ENV=production webpack --config webpack.plugin.js"
}
```

## Configuration Structure

### Development Configuration
```javascript
// webpack.dev.js
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'eval-source-map',
  devServer: {
    hot: true,
    open: true,
    port: 3000
  }
});
```

### Production Configuration
```javascript
// webpack.prod.js
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');

module.exports = merge(common, {
  mode: 'production',
  optimization: {
    minimizer: [
      new TerserPlugin(),
      new CssMinimizerPlugin()
    ]
  }
});
```

## Loader Configuration

### TypeScript Loader
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  }
};
```

### CSS and Sass Loaders
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.s?css$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader'
        ]
      }
    ]
  }
};
```

### Asset Loaders
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|svg)$/i,
        use: [
          {
            loader: 'file-loader',
            options: {
              outputPath: 'images/'
            }
          }
        ]
      }
    ]
  }
};
```

## Plugin Configuration

### HTML Plugin
```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      filename: 'index.html'
    })
  ]
};
```

### CSS Extraction Plugin
```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css'
    })
  ]
};
```

### Copy Plugin
```javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
  plugins: [
    new CopyWebpackPlugin({
      patterns: [
        {
          from: 'public',
          to: 'public'
        }
      ]
    })
  ]
};
```

## Optimization Strategies

### Code Splitting
```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        },
        common: {
          name: 'common',
          minChunks: 2,
          chunks: 'all',
          enforce: true
        }
      }
    }
  }
};
```

### Tree Shaking
```javascript
module.exports = {
  optimization: {
    usedExports: true,
    sideEffects: false
  }
};
```

### Bundle Analysis
```javascript
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin({
      analyzerMode: 'static',
      openAnalyzer: false
    })
  ]
};
```

## Development Server Configuration

### Hot Module Replacement
```javascript
module.exports = {
  devServer: {
    hot: true,
    liveReload: false,
    historyApiFallback: true,
    static: {
      directory: path.join(__dirname, 'public')
    }
  }
};
```

### Proxy Configuration
```javascript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
};
```

## Environment Variables

### Environment Configuration
```javascript
const Dotenv = require('dotenv-webpack');

module.exports = {
  plugins: [
    new Dotenv({
      path: './.env',
      safe: true,
      allowEmptyValues: true
    })
  ]
};
```

### Define Plugin
```javascript
const webpack = require('webpack');

module.exports = {
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
      'process.env.API_URL': JSON.stringify(process.env.API_URL)
    })
  ]
};
```

## Performance Optimization

### Caching
```javascript
module.exports = {
  optimization: {
    moduleIds: 'deterministic',
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  }
};
```

### Compression
```javascript
const CompressionPlugin = require('compression-webpack-plugin');

module.exports = {
  plugins: [
    new CompressionPlugin({
      algorithm: 'gzip',
      test: /\.(js|css|html|svg)$/,
      threshold: 8192,
      minRatio: 0.8
    })
  ]
};
```

## Build Optimization

### Production Build
```javascript
module.exports = {
  mode: 'production',
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true,
            drop_debugger: true
          }
        }
      })
    ]
  }
};
```

### Source Maps
```javascript
module.exports = {
  devtool: process.env.NODE_ENV === 'production'
    ? 'source-map'
    : 'eval-source-map'
};
```

## Troubleshooting

### Common Issues

#### Build Failures
```bash
# Clear cache and rebuild
rm -rf node_modules/.cache
npm run build
```

#### Memory Issues
```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      maxSize: 244000
    }
  }
};
```

#### TypeScript Errors
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              transpileOnly: true
            }
          }
        ]
      }
    ]
  }
};
```

## Best Practices

### 1. Configuration Organization
- Separate development and production configs
- Use webpack-merge for common configuration
- Keep configuration files focused and maintainable

### 2. Performance
- Implement proper code splitting
- Use tree shaking effectively
- Optimize bundle sizes
- Implement caching strategies

### 3. Development Experience
- Configure hot module replacement
- Set up proper source maps
- Implement development server features
- Use environment variables effectively

### 4. Build Process
- Automate build processes
- Implement proper error handling
- Use build analysis tools
- Monitor build performance

## References

- [Webpack Documentation](https://webpack.js.org)
- [Webpack 5 Migration Guide](https://webpack.js.org/migrate/5/)
- [Webpack Performance](https://webpack.js.org/guides/performance/)
- [Guidelines Index](./README.md#guidelines-index)

Last updated: January 2025
