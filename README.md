# typesense-scraper

<a href="https://github.com/celsiusnarhwal/typesense-scraper/releases"><img alt="GitHub release (latest SemVer)" src="https://img.shields.io/github/v/release/celsiusnarhwal/typesense-scraper?color=orange&label=latest%20release&logo=github&style=for-the-badge"></a>


This GitHub Action will run [Typesense](https://typesense.org)'s [DocSearch Scraper](https://github.com/typesense/typesense-docsearch-scraper)
on your documentation site. This action works best with sites whose source code is on GitHub, but strictly speaking, 
it will work on any repository that has a DocSearch config file. What's that, you ask? We'll get to that in a moment.

## Usage

### Running Typesense

Follow [these instructions](https://typesense.org/docs/guide/install-typesense.html) to install and run Typesense.
Make note of your hostname(s) and API key(s); you'll need them later. If you use [Typesense Cloud](https://cloud.typesense.org),
your hostnames and API keys will be provided to you. If you self-host Typesense, your hostname is the public IP address 
of the server you're hosting Typesense on and your API key is whatever you set for the `TYPESENSE_API_KEY`parameter 
when you started Typesense.

### Running the Scraper

#### Creating the DocSearch Config File

First, create a DocSearch config JSON file in the root of your repository. This file can be named anything and placed
anywhere, but the action's default settings assume it's named `docsearch.config.json` and placed at the root of your
repository.

The easiest way to go about this is by taking [this config file](https://github.com/algolia/docsearch-configs/blob/master/configs/docusaurus-2.json),
which [Docusaurus](https://docusaurus.io/) once used for its own site, and editing it to point to your site. If you want to
tweak it further or write your config file from scratch, you can check out [Algolia's documentation](https://docsearch.algolia.com/docs/legacy/config-file/) 
for an exhaustive list of configuration options. (Typesense DocSearch is a fork of [Algolia DocSearch](https://github.com/algolia/docsearch) 
and uses the same configuration schema.)

#### Using the Action

First, store your Typesense API key as a secret in your repository. It doesn't matter what you name this secret.
(See [GitHub's documentation on secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).)

Then add the following to one of your repository's workflows:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v3   # You MUST checkout your repository first!

  - name: Run DocSearch Scraper
    uses: celsiusnarhwal/typesense-scraper@v2
    with:
      # The secret containing your Typesense API key. Required.
      api-key: ${{ secrets.TYPESENSE_API_KEY }}
      
      # The hostname or IP address of your Typesense server. Required.
      host: xxx.a1.typesense.net
      
      # The port on which your Typesense server is listening. Optional. Default: 8108.
      port: 8108
      
      # The protocol to use when connecting to your Typesense server. Optional. Default: http.
      protocol: http
      
      # The path to your DocSearch config file. Optional. Default: docsearch.config.json.
      config: docsearch.config.json
```

That's all there is to it.

# License

typesense-scraper is released under the [MIT License](LICENSE.md).