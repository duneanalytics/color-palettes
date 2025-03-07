# Color Palettes

## Overview

Color Palettes is a community-maintained collection of standardised brand colors for blockchain protocols, chains, and tokens. This tool helps analysts maintain consistent color schemes across their visualisations without manual color picking for each chart.

## Why Use Color Palettes?

When creating visualisations on Dune, analysts frequently work with the same protocols and chains across different charts. Manually selecting colors for each visualisation is:

- Time-consuming
- Prone to inconsistency
- Difficult to maintain across multiple dashboards

This standard provides a single source of truth for protocol brand colors, ensuring consistency and saving valuable time.

## Schema

The color palettes are maintained in a JSON format with the following structure:

```json
{
  "protocol_name": {
    "hex": "#color_code",
    "aliases": ["alias1", "alias2"]
  }
}
```

### Schema Properties

| Property      | Type   | Description                                              |
| ------------- | ------ | -------------------------------------------------------- |
| protocol_name | string | The official name of the protocol or chain               |
| hex           | string | The hex color code of the protocol's primary brand color |
| aliases       | array  | Common variations of the protocol name used in queries   |

### Example Schema

```json
{
  "Ethereum": {
    "hex": "#627EEA",
    "aliases": ["eth", "weth", "ethereum"]
  },
  "Polygon": {
    "hex": "#8247E5",
    "aliases": ["matic", "polygon"]
  },
  "Uniswap": {
    "hex": "#FF007A",
    "aliases": ["uni", "uniswap_v2", "uniswap_v3"]
  }
}
```

## Usage in Dune Queries

### Automatic Application

Color Palettes are automatically applied to your visualisations when protocols or chains are detected in your query results, as long as they match any of the aliases on the schema (lowercased). This means you don't need to manually specify colors in most cases.

**A pratical example:**

Considering the following schema

```json
{
  "Ethereum": {
    "hex": "#82828",
    "aliases": ["eth", "weth"]
  }
}
```

the following examples would match

- Eth
- eth
- WeTh
- `evm_<color:eth>`
  - we added this special case, so you can still add append other strings to the results and still have the color matched

but these would not

- ethereum
- evm<clor:eth>
- color:eth

### Customisation Options

You have full control over your visualisation colors:

1. **Override Colors**:
   - You can override any automatically applied color with your preferred choice
2. **Restore Default Colors**:
   - Easily revert back to the standard Color Palettes at any time with the `Restore colors` button

## Contributing

The Color Palettes is a community-driven project. To contribute:

1. Submit new protocol colors
2. Update existing color codes
3. Add new aliases for existing protocols
4. Report inconsistencies

## Handling Alias Conflicts

### What is an Alias Conflict?

An alias conflict occurs when the same alias is associated with multiple protocols. For example:

- "BSC" could refer to "Binance Smart Chain" or "Basis Cash"
- "LINK" could refer to "Chainlink" or "Link Protocol"

### Automated CI Checks

The repository includes automated checks that run on every pull request to ensure the integrity of the color palette system.

#### What Gets Checked

- Duplicate aliases across different protocols
- Valid hex color codes
- JSON schema validation
- Required fields presence

### Example CI Failure

```json
{
  "Binance Smart Chain": {
    "hex": "#F3BA2F",
    "aliases": ["bsc", "bnb"]
  },
  "Basis Cash": {
    "hex": "#000000",
    "aliases": ["bsc", "basis"] // CI will fail: "bsc" alias already exists
  }
}
```

### Resolution Process

1. CI job detects the conflict
2. Pull request is blocked from merging
3. Author must resolve by:
   - Removing the conflicting alias
   - Using a more specific alias
   - Discussing in the PR if there's a special case

### Best Practices for Aliases

1. **Before Creating a PR**
   - Check existing protocols and their aliases
   - Use specific, unambiguous aliases
   - Include common variations of the name
2. **When CI Fails**
   - Review the CI logs for specific conflict details
   - Update your changes accordingly
   - Engage in discussion if unclear

## Best Practices

- Always use the official brand colors when available
- Maintain consistent colors across related dashboards
- Use aliases to handle common protocol name variations
- Consider color accessibility in your visualisations
