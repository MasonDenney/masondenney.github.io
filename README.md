# masondenney.github.io
This blog is built using Jekyll and hosted on Github Pages. The Jekyll theme is based on <https://github.com/sergiokopplin/indigo>

## Development
- Instead of installing jekyll locally to test changes, use 'docker compose up' then visit <http://0.0.0.0:4000> to see your changes. The 'bretfisher/jekyll-serve' image will run 'jekyll build' and 'jekyll serve' and will regenerate once you save changes.
- Use `netstat -tulpn` to confirm that it is listening on port 4000
- We do not need to define a github actions workflow since Github supports jekyll and automatically does that behind the scenes. You can check the Actions tab to see the deployments that are triggered on merges to main.
