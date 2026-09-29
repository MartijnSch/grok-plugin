---
name: outreserve-travel
description: Search and compare OutReserve campgrounds or US RV rentals, open listing details, and create or inspect campground availability alerts. Use when the user mentions OutReserve or wants OutReserve inventory; do not use for unrelated travel research or other booking platforms.
metadata:
  author: OutReserve
  short-description: Find OutReserve campgrounds and RV rentals
---

# OutReserve travel

Help the user reach a useful OutReserve result: a short list of relevant inventory, a verified listing detail, or a campground availability alert they explicitly requested.

## Route the request

- Use `search_campgrounds` for campgrounds, RV parks, tent sites, cabins, or glamping stays.
- Use `search_rvs` for motorhomes, camper vans, travel trailers, or other US RV rentals.
- Use `campground_details` or `rvs_detail` when the user asks about one known result. Prefer the UUID returned by a search over a name lookup.
- Use `get_campground_search_filters` only when the request needs exact supported amenity or filter values.
- Use `my_availability_alerts` or `availability_alert_status` only for the signed-in user's alerts.
- Use `create_availability_alert` only after the user has clearly asked to create one and the campground UUID plus exact start and end dates are known. Do not retry a successful or uncertain create call.

## Search faithfully

- Pass both dates together in `YYYY-MM-DD` form. Resolve relative dates against the current date and state the resolved dates in the answer when ambiguity could matter.
- Preserve the user's constraints. Do not silently broaden the place, dates, site type, RV class, price, sleeping capacity, pet policy, delivery, or amenity requirements.
- Start with a small result set. Expand or paginate only when the first results are insufficient.
- Use `language` and `measurementSystem` when the user states a preference; otherwise let OutReserve return its canonical presentation.
- Never invent a price, rating, amenity, policy, booking method, availability state, or URL. Use only the tool payload.

## Availability and booking boundaries

- For dated campground results, treat `availableUnitCount` as the availability signal and surface any warnings. A matching campground with zero available units is not available for those dates.
- RV rental date inputs provide trip context; they do not prove live availability. Say that availability must be confirmed on the linked listing.
- A displayed rate is not a guaranteed trip total. Describe it with the label and currency returned by OutReserve and direct the user to the canonical listing for final availability, fees, taxes, and checkout.
- Prefer `canonicalUrl` for the next step. If the payload exposes only a provider `sourceUrl`, label it as the provider listing.

## Present the result

Give the user a concise comparison using the fields most relevant to their request, followed by the best OutReserve link. Mention material warnings such as seasonal closure or unconfirmed availability. If no exact result matches, say which constraint prevented a match before offering a narrower or broader follow-up search.
