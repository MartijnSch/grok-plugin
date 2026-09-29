# OutReserve plugin for Grok Build

Find campgrounds, RV parks, cabins, glamping stays, and US RV rentals from Grok Build. The plugin connects to OutReserve's hosted Model Context Protocol (MCP) server and adds routing guidance for accurate search, listing-detail, and availability-alert workflows.

## Included components

- Hosted MCP connection: `https://outreserve.com/apps/mcp`
- `outreserve-travel` skill for choosing the correct OutReserve tool and presenting inventory faithfully
- No local executable code, install scripts, lifecycle hooks, or bundled credentials

The public search and detail tools do not require an OutReserve account. Availability-alert tools use OAuth 2.0 with PKCE. Grok opens OutReserve sign-in in the browser when an authenticated tool is first used; users should never paste an access token into chat.

## Capabilities

| Tool | Authentication | Purpose |
| --- | --- | --- |
| `search_campgrounds` | None | Search campgrounds and outdoor stays by place, dates, site type, amenities, pets, and season |
| `get_campground_search_filters` | None | Read the exact supported campground filters |
| `campground_details` | None | Open one campground by UUID or name |
| `search_rvs` | None | Search US RV rentals by place, class, price, capacity, make/model, delivery, pets, and more |
| `rvs_detail` | None | Open one RV rental by UUID/external ID or name |
| `my_availability_alerts` | OutReserve OAuth | List the signed-in user's campground alerts |
| `availability_alert_status` | OutReserve OAuth | Check one alert's current status |
| `create_availability_alert` | OutReserve OAuth | Watch a campground for openings across exact dates |

## Test locally

With Grok Build installed, validate and install this repository:

```bash
grok plugin validate .
grok plugin install . --trust
```

Start a new Grok session or reload plugins, open `/mcps`, and confirm `outreserve` is ready. Public tools can be exercised immediately. Grok will prompt for OutReserve OAuth only when an availability-alert tool needs it.

Example prompts:

- `Find pet-friendly RV campgrounds near Yosemite for October 10-13.`
- `Compare Class B camper vans near Seattle that sleep two.`
- `Show me details for the first campground.`
- `Create an availability alert for those campground dates.`

## Marketplace submission

The official xAI marketplace accepts third-party plugins from a public, reachable repository and pins every remote source to a full commit SHA. This repository is the public source for the OutReserve plugin. Add an entry like this to `xai-org/plugin-marketplace`:

```json
{
  "name": "outreserve",
  "description": "Search OutReserve campgrounds and US RV rentals, inspect listing details, and manage signed-in campground availability alerts through the hosted OutReserve MCP server.",
  "category": "travel",
  "source": {
    "source": "url",
    "url": "https://github.com/MartijnSch/grok-plugin.git",
    "sha": "FULL_40_CHARACTER_LOWERCASE_COMMIT_SHA"
  },
  "homepage": "https://outreserve.com/developers/mcp",
  "keywords": [
    "outreserve",
    "outreserve campgrounds",
    "outreserve rv rentals"
  ],
  "domains": [
    "outreserve.com"
  ]
}
```

Regenerate the marketplace component index, run the marketplace validator, and submit the catalog change for xAI review.

## Network, authentication, and data use

The plugin contacts only these OutReserve-owned endpoints:

- `https://outreserve.com/apps/mcp` for MCP requests
- `https://outreserve.com/.well-known/oauth-protected-resource/apps/mcp` for protected-resource discovery
- `https://outreserve.com/.well-known/oauth-authorization-server` for OAuth discovery
- `https://outreserve.com/oauth/register` for rate-limited Grok public-client registration
- `https://outreserve.com/oauth/authorize`, `/oauth/token`, and `/oauth/revoke` for OAuth lifecycle requests

Search inputs and tool arguments are sent to OutReserve to fulfill the request. Authenticated alert tools receive the signed-in user's OutReserve access token through Grok's MCP credential flow. The plugin does not read local files, environment variables, SSH keys, or unrelated credentials.

Use of OutReserve is governed by the [OutReserve Terms of Service](https://outreserve.com/terms-of-service) and [Privacy Policy](https://outreserve.com/privacy-policy).

## License

Copyright OutReserve. The plugin metadata and skill are proprietary under the included [license](LICENSE). Use of the hosted service is governed by OutReserve's terms.
