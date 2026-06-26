# 🛒 ShardShop

> **A professional dual-currency shop plugin for Paper 1.21.4**  
> Made by **KIBA OFFICIAL**

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 💎 **Dual Currency** | **Shards** (digital, database-only) + **Coins** (Vault economy) |
| 🏪 **Shop GUI** | Clean 54-slot GUI with player head, live balance, and 11 shop items |
| 🏷️ **Auction House** | Full paginated auction house — list, browse, buy, cancel, and auto-claim |
| 💰 **Sell System** | Sell hand, all, or via GUI with per-material price config |
| 🔁 **Currency Convert** | Convert Coins → Shards at a configurable exchange rate |
| 💤 **AFK Rewards** | Earn Shards while AFK; action-bar countdown; movement detection |
| ⛏️ **3×3 Miner** | Custom enchantment — pickaxe mines a 3×3 area |
| 🌲 **Tree Feller** | Custom enchantment — axe fells an entire tree instantly |
| 🪣 **3×3 Digging** | Custom enchantment — shovel digs a 3×3 area |
| 🛡️ **Stackable Totems** | Stack multiple Totems of Undying in one item slot |
| 📊 **PlaceholderAPI** | Full PAPI expansion for scoreboards and chat |
| 🌐 **WorldGuard Support** | Custom enchants respect region protection |
| 🗄️ **SQLite / MySQL** | HikariCP connection pool, async writes, zero data loss |
| 🔒 **Exploit-Proof GUIs** | All inventory exploits blocked (shift-click, drag, dupe, etc.) |

---

## 📦 Requirements

| Dependency | Required? | Notes |
|------------|-----------|-------|
| **Paper 1.21.4** | ✅ Yes | Must be Paper (not Spigot) |
| **Java 21** | ✅ Yes | Included in modern Paper servers |
| **Vault** | ⚠️ Recommended | Required for `/coins` and `/sell` |
| **Economy plugin** | ⚠️ Recommended | EssentialsX, CMI, etc. |
| **PlaceholderAPI** | 🔵 Optional | For placeholders |
| **WorldGuard** | 🔵 Optional | For region-aware enchants |

---

## 🚀 Installation

1. Download `ShardShop-1.0.0.jar`
2. Drop it into your server's `plugins/` folder
3. Start (or restart) the server — configs generate automatically
4. Edit `plugins/ShardShop/config.yml` to your liking
5. Reload with `/shardshop reload` or restart

> ✅ **No extra libraries needed.** SQLite, MySQL connector, and HikariCP are all bundled inside the JAR.

---

## ⚙️ Commands

### Shop & Currency
| Command | Permission | Description |
|---------|-----------|-------------|
| `/shardshop` | `shardshop.use` | Open the shop GUI |
| `/shardshop reload` | `shardshop.admin` | Reload all configs |
| `/sell hand` | `shardshop.sell` | Sell item in your hand |
| `/sell all` | `shardshop.sell` | Sell all sellable items |
| `/sell gui` | `shardshop.sell` | Open the sell GUI |
| `/convert <amount>` | `shardshop.convert` | Convert Coins → Shards |

### Economy Management
| Command | Permission | Description |
|---------|-----------|-------------|
| `/shards balance [player]` | `shardshop.shards` | Check shard balance |
| `/shards give <player> <amt>` | `shardshop.admin` | Give shards |
| `/shards take <player> <amt>` | `shardshop.admin` | Remove shards |
| `/shards set <player> <amt>` | `shardshop.admin` | Set shard balance |
| `/coins balance [player]` | `shardshop.coins` | Check coin balance |
| `/coins give <player> <amt>` | `shardshop.admin` | Give coins (via Vault) |
| `/coins take <player> <amt>` | `shardshop.admin` | Remove coins |
| `/coins set <player> <amt>` | `shardshop.admin` | Set coin balance |

### Auction House
| Command | Permission | Description |
|---------|-----------|-------------|
| `/ah` | `shardshop.auction` | Browse the auction house |
| `/ah sell <price>` | `shardshop.auction` | List held item for sale |

### AFK
| Command | Permission | Description |
|---------|-----------|-------------|
| `/afk` | `shardshop.afk` | Toggle AFK mode |

---

## 💲 Placeholders (PlaceholderAPI)

| Placeholder | Returns |
|-------------|---------|
| `%shardshop_shards%` | Player's shard balance |
| `%shardshop_coins%` | Player's coin balance (formatted) |
| `%shardshop_afk%` | `true` / `false` |
| `%shardshop_afk_shards%` | Shards earned per AFK cycle |
| `%shardshop_conversion_rate%` | Coins required per shard |

---

## 🔧 Configuration Overview

### `config.yml` — Key Sections

```yaml
database:
  type: sqlite          # sqlite or mysql
  host: localhost
  port: 3306
  database: shardshop
  username: root
  password: ""

economy:
  coins-per-shard: 2000  # Conversion rate

afk:
  enabled: true
  shards-per-minute: 2
  max-afk-minutes: 60
  check-interval-seconds: 60

auction:
  max-listings-per-player: 10
  default-expiry-hours: 24

# Item prices — edit to your needs
prices:
  netherite_sword: 500
  netherite_pickaxe: 750
  # ... etc
```

### `messages.yml`

All player-facing messages are in `messages.yml` with full color support (`&a`, `&#RRGGBB`) and `{placeholder}` tokens.

---

## 🎮 Custom Enchantments

Custom enchantments are applied to items using a PDC (Persistent Data Container) tag. Use the admin command or your preferred item editor to set:

| Enchantment | PDC Value | Tool | Effect |
|-------------|-----------|------|--------|
| **3×3 Miner** | `miner_3x3` | Pickaxe | Mines a 3×3 area around the broken block |
| **Tree Feller** | `tree_feller` | Axe | Fells an entire connected tree (up to 256 logs) |
| **3×3 Digging** | `digging_3x3` | Shovel | Digs a 3×3 area around the broken block |

All enchantments:
- Fire individual `BlockBreakEvent`s per block → WorldGuard regions are respected
- Have a re-entry guard → no infinite recursion
- Never break bedrock, barriers, containers, or other protected materials
- Respect Fortune and Silk Touch on the tool

---

## 🏦 Auction House Guide

1. Hold the item you want to sell
2. Run `/ah sell <price>` — item is taken from your hand
3. Other players browse with `/ah` and left-click to buy
4. Coins go into your **unclaimed payouts** (click **Claim** in the GUI)
5. Unsold listings expire after the configured time and appear in **Claim** as item returns

---

## 🔒 Security

ShardShop was built with a security-first mindset:

- **GUI exploit prevention** — every `InventoryClickEvent`, drag, number-key, shift-click, and creative dupe is cancelled before any processing
- **Atomic shard purchases** — uses `ConcurrentHashMap.compute()` for true atomicity; two concurrent purchases cannot both succeed if only one can be afforded
- **Non-mutating inventory checks** — uses `InventoryUtil.canFit()` instead of `addItem()` for space checks, preventing free-item exploits
- **Transaction ordering** — coins deducted before item given; payout recorded before listing removed; items delivered before DB rows marked claimed
- **Race-condition guards** — per-player purchasing lock on auction buys prevents double-click exploits

---

## 📁 File Structure

```
plugins/ShardShop/
├── config.yml          # All prices, AFK settings, DB config
├── messages.yml        # All player-facing messages
└── shardshop.db        # SQLite database (auto-created)
```

---

## 🐛 Support & Credits

**Made by KIBA OFFICIAL**

---

*ShardShop v1.0.0 — Built for Paper 1.21.4*
