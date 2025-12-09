---
name: dartsass-compile
description: Compile specific SCSS stylesheets using dartsass-rails. Use this when adding new stylesheets or updating the dartsass build configuration to rails applications.
---

# DartSass Rails Stylesheet Compiler

Use this skill to compile SCSS stylesheets in a Rails application using dartsass-rails.

## When to Use This Skill

- Adding a new client-specific stylesheet
- Testing stylesheet compilation after changes
- Troubleshooting dartsass build issues
- Verifying all configured stylesheets build correctly

## Instructions

### 1. Check Current Configuration

First, read the dartsass configuration to see what stylesheets are currently configured:

```ruby
# Check config/initializers/dartsass.rb or config/initializers/assets.rb
Rails.application.config.dartsass.builds
```

The configuration should be a hash like:
```ruby
{
  "application.scss" => "application.css",
  "client1.scss" => "client1.css",
  "client2.scss" => "client2.css"
}
```

### 2. Add New Stylesheet (if needed)

If adding a new client stylesheet:

1. Create the SCSS file in `app/assets/stylesheets/`
2. Add an entry to the dartsass.builds configuration:
   ```ruby
   Rails.application.config.dartsass.builds = {
     "application.scss" => "application.css",
     "existing.scss" => "existing.css",
     "newclient.scss" => "newclient.css"  # Add this line
   }
   ```
3. Restart the Rails server if running (initializers are loaded at startup)

### 3. Configure Procfile for Auto-Build (Optional but Recommended)

**IMPORTANT**: Before proceeding with manual compilation, check if the user wants to configure automatic builds in `Procfile.dev`.

Read the current `Procfile.dev` and check if it includes an initial build step:

```bash
cat Procfile.dev
```

**Ask the user**: "Would you like me to update the Procfile.dev to automatically build stylesheets on server startup?"

If yes, update the web process to run `dartsass:build` before starting the server:

```
web: bin/rails dartsass:build && bin/rails server -p 3000
css: bin/rails dartsass:watch
```

This ensures:
- Stylesheets are built fresh every time the dev server starts
- Server only starts if the build succeeds (using `&&`)
- Watch mode continues to rebuild on changes during development

### 4. Compile Stylesheets Manually

Run the build command:
```bash
bin/rails dartsass:build
```

This compiles all configured SCSS files from `app/assets/stylesheets/` to `app/assets/builds/`.

### 5. Verify Output

Check that CSS files were generated:
```bash
ls -la app/assets/builds/
```

You should see `.css` files corresponding to your `.scss` source files.

### 6. Check Compilation Errors

If compilation fails:
- Check SCSS syntax in the source file
- Verify file paths in the configuration
- Look for missing imports or undefined variables
- Run with trace for more details: `bin/rails dartsass:build --trace`

## Watch Mode (Development)

During development, use watch mode to automatically recompile on changes:
```bash
bin/rails dartsass:watch
```

This is typically configured in `Procfile.dev`:
```
web: bin/rails server -p 3000
css: bin/rails dartsass:watch
```

## Configuration Locations

DartSass configuration can be in either:
- `config/initializers/dartsass.rb` (dedicated file, recommended)
- `config/initializers/assets.rb` (combined with other asset config)

## Architecture Notes

In this multi-tenant CMS:
- Each client has their own stylesheet (e.g., `freeski.scss`, `davekinkead.scss`)
- Client views load their specific stylesheet via the `@domain` object
- Shared styles live in `application.scss`
- All stylesheets are compiled to `app/assets/builds/` for serving

## Examples

### Example 1: Add a new client stylesheet

```ruby
# 1. Create app/assets/stylesheets/newclient.scss
* {
  color: blue !important;
}

# 2. Update config/initializers/dartsass.rb
Rails.application.config.dartsass.builds = {
  "application.scss" => "application.css",
  "freeski.scss" => "freeski.css",
  "davekinkead.scss" => "davekinkead.css",
  "newclient.scss" => "newclient.css"  # Add this
}

# 3. Compile
bin/rails dartsass:build

# 4. Verify
ls app/assets/builds/newclient.css
```

### Example 2: Troubleshoot compilation

```bash
# Run with trace to see detailed output
bin/rails dartsass:build --trace

# Check the source file exists
ls app/assets/stylesheets/problematic.scss

# Verify configuration
cat config/initializers/dartsass.rb
```

### Example 3: Development workflow

```bash
# Start watch mode (automatically recompiles on save)
bin/rails dartsass:watch

# Or use Procfile for full dev environment
bin/dev  # Starts both web server and CSS watcher
```

## Common Issues

**Issue**: New stylesheet not compiling
- **Solution**: Restart Rails server (initializers load at startup)

**Issue**: CSS file not found in browser
- **Solution**: Check `app/assets/builds/` was generated, verify asset pipeline includes the file

**Issue**: Syntax errors
- **Solution**: Validate SCSS syntax, check for missing semicolons or braces

**Issue**: Import errors
- **Solution**: Verify imported file paths, ensure files exist in `app/assets/stylesheets/`


**ALWAYS** remind the user how to do it when you do it - help them learn!
