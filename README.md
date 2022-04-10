# x_collection_metadata

An extensible Solana NFT collection metadata pseudo-standard.

`x_collection_metadata` reuses the existing JSON NFT metadata format to add collection-specific attributes to a Metaplex [collection](https://docs.metaplex.com/token-metadata/specification#collections) mint.

This pseudo-standard would allow data consumers such as markets, rarity sites, tools, etc to automatically process on-chain collections.

## Rationale

The new Metaplex collection standard solves the problem of how to “tag” mints in a belongs_to relationship to a parent object.

This allows mints from a variety of sources to belong to one collection on-chain. E.g., mints from multiple Candy Machines, launchpads, auction platforms, custom minters etc can all have the same verified collection address.

The standard also allows creators to delegate collection “tagging” authority to other parties, and the verified creator address at index 0 no longer needs to be the same on all mints.

As the object is just a NFT mint, we can take advantage of the standard JSON NFT data format to attach collection-specific data to the mint on-chain, allowing creators to have a single source of collection “truth” in a permanent store where consumers like markets, tools, rarity sites, etc can query the blockchain for all collection information.

## Data Format

### Root object fields (Metaplex NFT JSON standard)

| Field      | Description | Required | Example |
| ----------- | ----------- | ----------- | ----------- |
| `name`      | The name of the collection       | Yes | `Friendly Frogs` |
| `description`   | A description of the collection        | Yes | `2,121 Friendly Frogs bringing friendship to the metaverse and protecting the Positivity Pond from bad vibes!` |
| `image`   | An image representing the collection        | No | `https://arweave.net/t65orC3i_dzdsXmtoa5wC6ZQkCRAeiwM9igf4xK-K2U` |

### Attributes

| Field      | Description | Required | Example |
| ----------- | ----------- | ----------- | ----------- |
| `x_collection_metadata.version` | The version of the `x_collection_metadata` object. Used to denote backwards-incompatible changes in the data format. | Yes | `1` |
| `id` | A pseudo-unique ID for the collection for consumers to use in URLs. Must match a `[a-z0-9_]` regex. | Yes | `friendly_frogs` |
| `supply` | The total number of mints in the collection from all sources. | Yes | `2121` |
| `mints` |  A pointer to a URI containing the complete mint list for the collection. This should be a JSON array. | No | `https://moonrank.app/mint_list/friendly_frog_social_club` |
| `url.web` |  A link to the collection website. | No | `https://ffsc.io` |
| `url.twitter` |  A link to the collection Twitter account. | No | `https://twitter.com/FriendlyFrogSC` |
| `url.discord` |  A link to the collection Discord. | No | `https://discord.gg/friendlyfrogs` |
| `market.[market].id` |  A marketplace collection ID. | No | `friendly_frog_social_club` |

## Example JSON

```
{
  "name": "Friendly Frogs",
  "description": "2,121 Friendly Frogs bringing friendship to the metaverse and protecting the Positivity Pond from bad vibes!",
  "image": "https://arweave.net/t65orC3i_dzdsXmtoa5wC6ZQkCRAeiwM9igf4xK-K2U",
  "attributes": [
    {
      "trait_type": "x_collection_metadata.version",
      "value": "1"
    },
    {
      "trait_type": "id",
      "value": "friendly_frogs"
    },
    {
      "trait_type": "supply",
      "value": "2121"
    },
    {
      "trait_type": "mints",
      "value": "https://moonrank.app/mint_list/friendly_frog_social_club"
    },
    {
      "trait_type": "url.web",
      "value": "https://ffsc.io"
    },
    {
      "trait_type": "url.twitter",
      "value": "https://twitter.com/FriendlyFrogSC"
    },
    {
      "trait_type": "url.discord",
      "value": "https://discord.gg/friendlyfrogs"
    },
    {
      "trait_type": "market.magiceden.id",
      "value": "friendly_frog_social_club"
    }
  ]
}
```
