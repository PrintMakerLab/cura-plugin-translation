# Translate Cura plugin 

A github workflow action that creates and updates translation files for Cura plugin source repositories.

## Inputs

| Parameter               | Description                                                | Default  |
| ----------------------- | ---------------------------------------------------------- | -------- |
| `translation_folder`    | Path to the translation directory                          | `i18n`   |
| `translation_name`      | Name of the translation template and locale files          | ``       |
| `plugin_name`           | That name will be dispayed in the translation files header | ``       |

## Outputs

The following output values can be accessed via `${{ steps.<step-id>.outputs.<output-name> }}`:

| Name                    | Description                                            | Type          |
| ----------------------- | ------------------------------------------------------ | ------------- |
| `template_updated`      | 1 if .pot file was updated                             | bool          |
| `locales_updated`       | 1 if .mo file(s) was updated                           | bool          |


## Example

```yaml
name: "Translation with the action"

on:
  push:
    branches-ignore:
      - 'develop'
      - 'release-**'
      
jobs:
   update_translation:
     name: "Update plugin translation"
     runs-on: "ubuntu-latest"
     
     steps:
      - uses: actions/checkout@v3
      
      - name: "Translation"
        id: translation
        uses: PrintMakerLab/cura-plugin-translation@main
        with:
          translation_folder: 'i18n'
          translation_name: 'mksplugin'
          plugin_name: 'MKS WiFi Plugin'
   
      - if: ${{ steps.translation.outputs.template_updated || steps.translation.outputs.locales_updated }}
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "[CI] Translation updated"
```