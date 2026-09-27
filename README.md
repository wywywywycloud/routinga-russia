# RoutingA Russia

A concise RoutingA profile for v2rayA: send the Russian domain category directly, send known exceptions through a proxy, and use **`default: proxy`** for everything else.

**[Download the raw configuration](https://raw.githubusercontent.com/wywywywycloud/routinga-russia/main/routinga-ru.txt)** · [View the rules](routinga-ru.txt)

## Routing policy

Rules are evaluated from top to bottom:

1. **Proxy exceptions:** 36 domain suffixes from itdoginfo `Russia/inside` that overlap direct routes. They take priority over the Russian category.
2. **Russian category → direct:** `geosite:category-ru` includes Russian domain zones, Yandex, VK/Mail, banks, government services, shops, and related CDNs.
3. **Additional direct domains:** two entries from itdoginfo `Russia/outside`.
4. **Local hostnames and LAN IPs → direct:** `geosite:private` and `geoip:private`, matching v2rayA's `global` preset.
5. **Everything else → proxy.**

Only exceptions that need to override direct routes are listed explicitly. Other `Russia/inside` domains already match `default: proxy`. The `.ua` exception also overrides Ukrainian domains included in the Russian category. Where the upstream inside and outside lists conflict, inside takes precedence; for example, `showip.net` uses the default proxy route.

`direct` uses the device's normal ISP connection. It provides a Russian exit IP only when that connection already exits in Russia.

## Requirements and installation

- v2rayA with RoutingA support and a connected proxy available as `proxy`.
- A v2fly-compatible `geosite.dat` containing **`category-ru`** and **`private`**, plus `geoip.dat` containing **`private`**. The exceptions were calculated against the source snapshot below.
- For transparent traffic, enable traffic sniffing so the core can recognize domains when available.

1. Export your existing rules if you want a backup.
2. Open the main **Settings → RoutingA rules** editor. Import `routinga-ru.txt`, or replace the entire editor contents with the raw file, then save.
3. Set the rule port's routing mode to **RoutingA**.
4. If using transparent proxying, configure it to follow the rule port's routing mode.

Use the main RoutingA editor: a custom inbound's separate rule editor ignores `default:`.

## Updates and scope

This is a **static snapshot dated September 27, 2026**. It does not download or refresh lists automatically. Refresh the exceptions whenever you update the category: its membership can change.

The policy follows domain lists rather than testing live availability. Since `category-ru` includes entire domain zones, a newly blocked Russian domain can go direct until it is added as an exception. To override it, add `domain(domain: example.ru) -> proxy` above the category rule.

There is no `geoip:ru` rule. Traffic without a recognized domain uses the direct route if its destination matches `geoip:private`; otherwise it falls back to the proxy. With `IPOnDemand`, the private-IP rule can also match an IP resolved from a domain. Explicit proxy exceptions remain above the private rules. Built-in v2rayA rules added before user rules retain their priority.

## Sources and validation

| Source | Pinned revision | Commit date (UTC) |
| --- | --- | --- |
| [v2fly/domain-list-community: category-ru](https://github.com/v2fly/domain-list-community/blob/bcea25493ed28c387660fe49ce1ceb242d2efca0/data/category-ru) | `bcea25493ed28c387660fe49ce1ceb242d2efca0` | 2026-09-25 |
| [itdoginfo/allow-domains: Russia/inside](https://github.com/itdoginfo/allow-domains/blob/949e4dee2e5285156755884994b6b6c1659828b4/Russia/inside-raw.lst) and [Russia/outside](https://github.com/itdoginfo/allow-domains/blob/949e4dee2e5285156755884994b6b6c1659828b4/Russia/outside-raw.lst) | `949e4dee2e5285156755884994b6b6c1659828b4` | 2026-09-21 |

The profile passed the actual **RoutingA v1.0.2** parser: 12 explicit rules and one final `default: proxy`. The proxy exceptions and Russian-category rules are unchanged from the original audited profile. The local-domain and LAN-IP rules use the same expressions as v2rayA's `global` preset.

The parser check validates configuration syntax. It does not verify the geodata installed on your device, live LAN routing, or reachability through your ISP.

## License and attribution

Original configuration and documentation are provided under the [MIT License](LICENSE). Source attribution and the preserved v2fly license notice are in [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
