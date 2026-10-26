# metabase-custom-maps

Public GeoJSON for Luminaria's Metabase custom maps. Metabase fetches custom-map geometry anonymously over
HTTPS, so these files have to live in a public place; this repository is that place, owned by the
`luminaria-tech` organisation.

**Do not edit the files here by hand.** They are copies of `DDL/dashboard/metabase/maps/` in the private
`luminaria-tech/data-processing` repository, where they are generated and reviewed. To change a map:
regenerate and merge it there, copy the files here, and point Metabase at the new commit.

| File | Contents |
|---|---|
| `spam_state_level_admin1.geojson` | SPAM state-level admin1 boundaries, 343 regions in 10 countries: USA 50, Brazil 27, India 36, Russia 74, Thailand 77, China 31, Germany 16, Poland 16, Australia 9, Pakistan 7 |
| `spam_state_level_<country>_admin1.geojson` | the same geometry split per country |
| `spam_state_level_admin1.mapping.json` | how each region was matched to `ods.country_state_codes` |

Each feature carries `region_key` (`<country_name>:<state_code>`, the key dashboards join on) and `state_name`.
Boundaries come from geoBoundaries ADM1 (open data).

| `usa_counties_final.geojson` | USA county boundaries (3 159 counties, US Census cartographic boundary file `gz_2010_us_050_00_500k`, public domain), keyed by `GEOID`; byte-for-byte copy of `msk01yo/Map/counties_final.geojson` — not generated in `data-processing` |

## Metabase

The combined file backs custom map `8d0a5763-dc84-f13f-467f-ea433d3df292` ("SPAM state-level admin1",
Admin → Settings → Maps) with `region_key = region_key` and `region_name = state_name`. Its URL is pinned to a
commit of this repository so a later push cannot change what dashboards render until Metabase is pointed at it.

The per-country files back the maps "SPAM state-level Brazil" (`638b8226…`), "SPAM state-level USA" (`7e07c679…`)
and "SPAM state-level Russia" (`472d0659…`), same field names. `usa_counties_final.geojson` backs the county map
"Final Map" (`8d9984e9-e50a-f48d-82f5-d86afc4b5546`, `region_key = GEOID`, `region_name = NAME`), used by the USA
county weather and USDA map cards.

History: until 2026-10-27 the state-level maps were served from a personal repository outside the organisation
(`Sindbag/metabase-custom-maps`, five countries, 264 regions; risk RSK-378). Moved here from
`luminaria-tech/data-processing@dd1e461a` (`DDL/dashboard/metabase/maps/`).
