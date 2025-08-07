# Cloud Native Buildpack for Chromium (amd64/arm64 compatible)

This buildpack downloads and installs Chromium to Linux amd64/arm64. It's useful for testing purposes combined with Playwright, Puppeteer (or similar) used in headless mode.
It's compatible with [Heroku FIR](https://www.heroku.com/blog/next-generation-heroku-platform/).

## Disclaimer

The author of this article makes any warranties about the completeness, reliability and accuracy of this information. **Any action you take upon the information of this website is strictly at your own risk**, and the author will not be liable for any losses and damages in connection with the use of the website and the information provided. **None of the items included in this repository form a part of the Heroku Services.**

## Prerequisites

- A Heroku account
- Heroku CLI installed
- pack installed (for local development)
- Docker installed (for local development)

## How to use

1. Create a new Heroku app (FIR)
2. add a `project.toml` at the root directoy and insert the following directives. Those installs chormium and its dependant libraries required at runtime:
   ```toml
   [[io.buildpacks.group]]
   id = "heroku/chromium"
   uri = "https://github.com/abernicchia-heroku/cnb-chromium-buildpack/releases/download/v1.0.0/cnb-chromium-buildpack-linux-arm64.cnb"

   [com.heroku.buildpacks.deb-packages]
   install = [
       # string version of a dependency to install
       "fonts-noto-color-emoji",
       "fonts-noto",
       "fonts-liberation",
       "fonts-ipafont-gothic",
       "fonts-wqy-zenhei",
       "fonts-thai-tlwg",
       "fonts-khmeros",
       "fonts-kacst",
       "fonts-freefont-ttf",
       "libxss1",
       "dbus",
       "dbus-x11",
       "tini",
       "poppler-utils",
       "poppler-data",
       "libatk1.0-0",
       "libatk-bridge2.0-0",
       "libdrm2",
       "libxkbcommon0",
       "libxcomposite1",
       "libxdamage1",
       "libxfixes3",
       "libxrandr2",
       "libgbm1",
       "libasound2t64",
       "libatspi2.0-0"
   ]

   [[io.buildpacks.group]]
   uri = "heroku/deb-packages"
   ```
4. Use CHROMIUM_VERSION to specify a chromium version. It can be set in the project.toml or as a Heroku env var (see https://devcenter.heroku.com/articles/build-time-config-vars).  Supported versions are listed here https://playwright.dev/docs/release-notes
4. Commit and push to Heroku

## Create a local OCI image
To vaidate if the build executes correctly
   ```bash
   pack build cnb-chromium-buildpack --buildpack . --builder heroku/builder:24
   ```

## Inspect the OCI image
To test and inspect the image
   ```bash
   docker run -it --rm --name my-app-cnb-chromium-buildpack --entrypoint bash cnb-chromium-buildpack
   ```

## Package the buildpack
To package the buildpack anytime is modified and then release it
   ```bash
   pack buildpack package cnb-chromium-buildpack --format file
   ```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## Support

For issues and questions, please open an issue in the GitHub repository.

