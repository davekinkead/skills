# Rails Setup

Complete setup for a new Rails 8 project including RSpec, FactoryBot, CI configuration, and Gemfile organization.

## Tasks to Perform

### 1. Install RSpec and FactoryBot

**Add gems to Gemfile** in the `group :development, :test` block:
```ruby
gem "rspec-rails", "~> 7.1"
gem "factory_bot_rails", "~> 6.4"
```

**Install dependencies:**
```bash
bundle install
```

**Generate RSpec configuration:**
```bash
bin/rails generate rspec:install
```

**Configure FactoryBot** in `spec/rails_helper.rb`:
Add inside the `RSpec.configure do |config|` block:
```ruby
# FactoryBot configuration
config.include FactoryBot::Syntax::Methods
```

**Verify installation:**
```bash
bundle exec rspec --version
```

### 2. Update CI Configuration for RSpec

**Update `config/ci.rb`:**
Replace the Rails test commands with RSpec:
```ruby
step "Tests: RSpec", "bundle exec rspec"
step "Tests: Seeds", "env RAILS_ENV=test bin/rails db:seed:replant"
```

Remove the separate system test step (RSpec runs all tests together).

**Update `.github/workflows/ci.yml`:**

In the `test` job, replace the test command:
```yaml
- name: Run tests
  env:
    RAILS_ENV: test
  run: bin/rails db:test:prepare && bundle exec rspec
```

Remove the separate `system-test` job entirely.

Move the screenshot artifact upload step into the main test job (after the test step):
```yaml
- name: Keep screenshots from failed system tests
  uses: actions/upload-artifact@v4
  if: failure()
  with:
    name: screenshots
    path: ${{ github.workspace }}/tmp/screenshots
    if-no-files-found: ignore
```

### 3. Organize Gemfile

**Rules:**
1. Keep `source "https://rubygems.org"` first
2. Keep `gem "rails"` declaration second
3. Sort all active gems alphabetically within each section:
   - Main gems (no group)
   - `group :development, :test`
   - `group :development`
   - `group :test`
   - `group :production`
4. Remove all descriptive comments
5. Keep commented-out gems (e.g., `# gem "bcrypt"`) at the end of their respective group
6. Preserve gem options like version constraints, `require: false`, `platforms:`, etc.

**Example structure:**
```ruby
source "https://rubygems.org"

gem "rails", "~> 8.1.1"

gem "bootsnap", require: false
gem "image_processing", "~> 1.2"
gem "importmap-rails"
gem "jbuilder"
gem "kamal", require: false
gem "propshaft"
gem "puma", ">= 5.0"
gem "solid_cable"
gem "solid_cache"
gem "solid_queue"
gem "stimulus-rails"
gem "sqlite3", ">= 2.1"
gem "thruster", require: false
gem "turbo-rails"
gem "tzinfo-data", platforms: %i[ windows jruby ]

# gem "bcrypt", "~> 3.1.7"

group :development, :test do
  gem "brakeman", require: false
  gem "bundler-audit", require: false
  gem "debug", platforms: %i[ mri windows ], require: "debug/prelude"
  gem "factory_bot_rails", "~> 6.4"
  gem "rspec-rails", "~> 7.1"
  gem "rubocop-rails-omakase", require: false
end

group :development do
  gem "web-console"
end

group :test do
  gem "capybara"
  gem "selenium-webdriver"
end
```

## Usage

When setting up a new Rails 8 project, perform all these tasks in order to:
- Replace default Rails testing with RSpec and FactoryBot
- Configure CI pipelines to use RSpec
- Create a clean, organized Gemfile
