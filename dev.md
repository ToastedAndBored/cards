# Dev env setup
Dev environment is defined in `flake.nix`.

То setup dev environment
- Use `nix develop` (or `direnv allow` for direnv users) to enter env defined in `flake.nix`.
- Copy `.env.example` file to `.env` and fill placeholder fields with actual secrets.
- Run `go get` to fetch go dependencies

# Running service
```sh
docker compose up --build
```

# Creating admin user
To promote user to admin, connect to DB after service start:
```sh
psql -h localhost -U devuser -d devdb
```
Find your user
```sql
SELECT * FROM users;
```
And change selected user type to 1
```sql
UPDATE users SET type=1 WHERE id=<YOUR USER ID>;
```

# Heroku
## Creating service
```sh
heroku apps:create web-cards-app --region eu
heroku open
```

## Setting env
```sh
heroku config:set KEY=value
heroku config
```

## Adding custom domain
```sh
heroku domains:add cards.moth.contact -a web-cards-app
heroku domains:wait 'cards.moth.contact'
```

```sh
heroku certs:auto:enable && heroku certs:auto --wait
heroku certs
```

## Monitoring
```sh
heroku ps
heroku logs --tail
```

## Releasing
Make shure app in consistent state
```sh
go mod tidy
go get
```

Setup git (if not yet)
```sh
heroku git:remote -a web-cards-app
```

Publish
```sh
git push heroku master
```

# TODO
## Fronend
- [ ] Setup service worker only when installing PWA
- [ ] Make avatars square
- [ ] Add matching svg icons to contacts in card component
- [ ] Repalce text at buttons on cards page winth svg icons
- [X] Place name of logged in user somewhere
- [X] Nav
  - [X] Same paddings for buttons in side nav
  - [X] Same buttons length in side nav
  - [X] Move buttons under the burger in side nav
  - [X] Remove main page button
  - [X] Use SVG logo instead of just text
- [ ] Style cards page
  - [ ] If there is no cards, add text block about it
  - [X] Add cards grid
  - [X] Replace "create new card" link button with `+` sign
- [ ] Style editor page
  - [ ] Add absolute position for preview
  - [X] Add live preview
  - [X] Make crop widget not so fucking big
  - [X] Style inputs (file inputs including)
- [ ] Style card page
- [ ] Style card not found page
- [ ] Style login page
  - [X] Block with list of "sign with" buttons
  - [X] Each button themed in related service colors and with logo
- [ ] Implement VCF file generation
- [ ] QR codes
  - [ ] Implement QR code with user provided logo generation
  - [ ] Generate two QR codes: one with link to card page and one with full VCF file (for offline usage)
- [ ] Favicon
  - [ ] Add default one
  - [X] For cards use avatar as favicon
- [ ] PWA
- [ ] Add footer
## Backend
- [ ] Add rate limiting
  - [ ] Check if file can exists via DB before going to S3
- [ ] Assume default locale by GeoIP & HTTP headers
- [X] Add localisation system
- [ ] Add VIP accounts system
- [ ] Payment systems integration
- [ ] Paid features
  - [ ] Custom names
  - [ ] More cards
  - [ ] More fancy QR codes
  - [ ] Offline mode
## Features
- [ ] Avatar & logo deletion
- [ ] Add personal site block
- [ ] Support for more sochial links in cards
- [ ] Card views counter
- [ ] Telegram previews
## Optimisation
- [ ] Scripts loading optimisation
  - [ ] Async
- [X] Media reencoding
- [X] Inmemory caching for S3 blobs
- [X] Add TTL & etags for static files
- [ ] Server push
- [ ] Static files
  - [ ] Cache them inmemory
  - [ ] Minify and/or comress them
  - [ ] Maybe use hashed names + long TTL
## Content
- [ ] Fill main page with something
  - Maybe gallery of published cards
- [ ] FAQ
- [ ] Tutorials
## DevOps
- [ ] Add Heroku publishing helpers
- [ ] Add GH actions bases on nix
## Pr
- [ ] Add video demostration to readme
## Etc
- [ ] GDPR and simmilar rules compliance
