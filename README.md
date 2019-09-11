# Auto Economy | README # 

AutoEconomy adapts the prices of every possible item to meet the server's constantly fluctuating supply and demand in real time. Effectively removing item abuse, as well as encouraging the purchase/sale of all available items within the game; and it does all of this through a comprehensive and customizable player experience.
I've put measureless time and meticulous thought into making this the best market economy simulator that it can be, and it means so much to see it appreciated by other interested folk. 

This README will hopefully clarify everything you need to know in order to make full use of all that AutoEconomy has to offer.
Of course, if you need any direct support, always feel free to contact me at any of the following locations:
> The official support server: https://discord.gg/TBQ3q3m *  
MC-Market: http://www.mc-market.org/members/194/  
Spigot: https://www.spigotmc.org/members/justisr.5901/  
Discord: justis#0194  
Email: justis.root@gmail.com

I'll do my best to resolve any questions you might have, thoroughly and promptly.


## Files and Folders ##

The file extension .hmff is intentional. Do not change the extensions.
This is my own file format I've developed for heightened flexibility, full comment support, paths with values, no need for string encapsulation, and faster loading/saving times.
If you use sublime, you may enjoy using a dedicated syntax: https://gist.github.com/justisr/6be4be9b7cd1bef1547c7ada26cdbc4d

From the root AutoEconomy plugin folder you will find the following files and folders:

| Location | Description |
| -------- | ----------- |
| config.hmff | This file is your main configuration, all of the most important settings are here. |
| messages.json | This file is your messages configuration. It's in JSON to provide complete support for chat click/hover event customization. |
| README.html | This file, explaining everything you need to know about how to go about setting up and maintaining this plugin. |
| logs/... | This folder contains all of the log files for all of the transactions that occur on your server. These logs are also used for rolling back player balances and market prices should you choose to roll back. |
| guis/... | This folder contains all of the configurations for all of the guis used by AutoEconomy. |
| guis/cart.hmff | This file defines the GUI settings for the cart GUI. |
| guis/order.hmff | This file defines the GUI settings for the order GUI. |
| guis/selldump.hmff | This file defines the GUI settings for the sell GUI. |
| guis/shop.hmff |  This file defines the main shop GUI and shop GUI settings for your server. |
| guis/shops/... | This folder contains all of the shops that your main shop will link to, also accessible through /shop shop-name. It will come with generated shop files depending on your server version. |
| market/... | This folder contains everything related to AutoEconomy's markets and the items and prices contained within. |
| market/groups/... | This folder contains all of the files for each market group you create, each file containing each group's market data. |
| market/item/... | This folder contains the configuration files for item specific settings on this server's markets. |
| .../item/aliases.hmff | This file contains aliases for all attainable items on the server. It will also allow you to modify the item's default display name within GUIS and commands. |
| .../item/custom.hmff | TODO: This file will allow you to add custom items to the server which either overwrite existing items or overwrite them if they contain particular data. |
| market/main/... | This folder contains the main market data, separated into three documented files for clarity. |
| .../main/groups.hmff | This file groups references for the concise determination of prices in prices.hmff |
| .../main/intangibles.hmff | This file contains price references for intangible entities such as "water" without a bucket, and "wool" without a color, etc.*|
| .../main/prices.hmff | This file contains the prices for all items on the server, utilizing references from within itself as well as intangibles.hmff and *.../groups.hmff* |
| userdata/... | This folder contains all the files containing each user's personal data. |


## Configurations ##

The names for items used in any AutoEconomy configuration file are not the names used in Bukkit's Material enum.
This is because not all of their enum names are particularly clear, and the names do not account for items with a shared material type but different byte values.
Using names independent of Bukkit also ensures that the names will work across all Minecraft/Bukkit versions. A full list of the item names is available in your *prices.hmff* file, which will generate a listing for all available items.

**GUI Configurations**

Even if not present in the file, all GUI inventories start out with the same default values for undefined slots. Any missing values from any of your slot types will use these defaults.

```
DefaultSettings: AIR
  Amount: 1
  Name:  
  Lore: 
  LeftClick: 
  RightClick: {LeftClick}
  MiddleClick: {LeftClick}
  ShiftLeftClick: {LeftClick}
  ShiftRightClick: {LeftClick}
```

These are changeable by including the paths in the file, and specifying a different value. Most GUI configurations will come with these defaults pre-generated for your ease of modification.
However, different GUI configurations have different behaviors. Ensure you're reading the documentation provided in each individual file to ensure that you're receiving the behavior you most desire.

Slot types extend these default values. You can create and use slot types for defining the settings of slots which you will use repeatedly in your configuration(s). Slots can then use the slot type as it is, or modify it further.

Creating a slot type is as simple as adding a name for the slot type, and then specifying the values that should override the default settings mentioned above. Any values not specified will inherit the DefaultSettings value.

```
SlotTypes:
  SlotTypeName1: POTATO
    Amount: 64
    Name: &7Bunch o' potatoes
  SlotTypeName2:
    RightClick: c/say This is a customized slot type.
  etc...
```

These slot types can then be referenced without indent, for defining which slots use and override that slot type. Slots which use but do not override the slot type, can be written as a value, separated by spaces. Slots which overide the slot type must be indented as the slot type's child, with the slot number as the key. Any values not specified will inherit the slot type's values.

```
SlotTypeName1: 6 12 24 32 42 49 50 51
  3:
    Amount: 1
    Name: &7Singular potato
SlotTypeName2: 0 1 2 4 7 8 9 11 13 14
  16: DIRT
  10: GRASS
    Name: &7This is custom slot number 10, with grass instead of AIR
    RightClick: c/say This is a customized slot.
```

The GUI inventory size will adjust to use the most rows needed to fit the largest defined slot. If you would like to make your inventory larger than you have items for, simply define a slot in your desired max row as AIR, and the inventory will accomidate it. Remember to start indexing your slots at 0.

The following placeholders are available for all string value fields in all GUI configurations:

| Placeholder | Description |
| ----------- | ----------- |
| {player} | The player viewing the GUI * |
| {item} | The AE name of the item |
| {slot} | The slot number |
| {name} | The item stack's display name, color codes omitted |
| {size} | The size of the item stack |
| {buyprice} | The buy price of the item |
| {buyprice:stack} | The buy price of a stack of the item |
| {buyprice:#} | The buy price of a custom amount of the item |
| {sellprice} | The sell price of the item |
| {sellprice:stack} | The sell price of a stack of the item |
| {sellprice:#} | The sell price of a custom amount of the item |
| {leftclick} | The left click action settings |
| {rightclick} | The right click action settings |
| {middleclick} | The middle click action settings |
| {shiftrightclick} | The shift right click action settings |
| {shiftleftclick} | The shift left click action settings |
> \* available for use in the inventory title

Some GUI configurations may offer additional placeholders, either for the inventory titles, or slots. These will be documented in their respective files. 
If you use a numerical placeholder as the variable of a "custom amount" placeholder, omit the brackets. For example: `{buyprice:size}`

It's advised that you not directly or indirectly reference a click action within itself, to avoid unexpected duplication.
To make a click execute a command action with the player as the sender, use `p/`
To make a click execute a command action with the console as the sender, use `c/`
Multiple actions can be performed within a single click by separating them with a semicolon. `;`

Player and console command actions are available in all GUI inventory configurations, however, some configurations may offer additional actions. These will be documented in their respective files.
Any unrecognized actions will be ignored.

To create multiple lore lines, use `\n` to indicate a new text line.

## PlaceholderAPI ##

AutoEconomy supports PlaceholderAPI, and also offers the following placeholders:

| Placeholder | Description |
| ----------- | ----------- |
| %autoecon_balance% | The player's monetary balance |
| %autoecon_buyprice_{item}% | The buy price of an item |
| %autoecon_buyprice_{item}:stack% | The buy price of a stack of an item |
| %autoecon_buyprice_{item}:{amt}% | The buy price of a custom amount of an item |
| %autoecon_sellprice_{item}% | The sell price of an item |
| %autoecon_sellprice_{item}:stack% | The sell price of a stack of an item |
| %autoecon_sellprice_{item}:{amt}% | The sell price of a custom amount of an item |

When customizing your GUI inventories, always prefer the GUI dedicated placeholders over PlaceholderAPI placeholders. AutoEconomy parses all of its GUI placeholders once, at loadup, and maps them for very fast use and rereuse. PlaceholderAPI placeholders requires parsing every time that they are used, making them much slower, and best suited for chat messages.


## Permissions ##

Remember that adding/negating a parent permission will add/negate all child permissions except where the child permission has been explicitly negated/added.
*autoecon.admin* defaults to OP players.
*autoecon.user* defaults to all players.

| Node | Parent | Description |
| ---- | ------ | ----------- |
| autoecon.admin | * | Admin permission group |
| autoecon.user | N/A | Primary user permission group |
| autoecon.trade | autoecon.user | Allows access to the /trade command |
| autoecon.shop | autoecon.user | Allows access to the /shop command |
| autoecon.sell | autoecon.user | Allows access to the /sell command |
| autoecon.sell.hand | autoecon.sell | Allows access to the /sell hand subcommand |
| autoecon.pay | autoecon.user | Allows access to the /pay command |
| autoecon.bal | autoecon.user | Allows access to the /bal command |
| autoecon.bal.other | autoecon.admin | Allows access to the /bal {user} command |
| autoecon.cart | autoecon.shop | Allows access to the /cart command |
| autoecon.order | autoecon.shop | Allows access to the /order command |
| autoecon.market.main | autoecon.user | Allows access to the main market |
| autoecon.market.private | N/A | Allows access to a private market |
| autoecon.market.group.X | N/A | Allows access to the market group X |

## Commands ##

The following is a table of all available commands. Sub-commands are also included in this tabel. Sub-commands can only be accessed through the use of their parent command (check usage).
Usage key: *{required}, [optional], -flag, either|or*

| Label | Aliases | Description | Usage |
| ----- | ------- | ----------- | ----- |
| autoecon | ae, autoeconomy, eco, econ, economy, auto, autoe, aecon | Main command for AutoEconomy | /autoecon [tab] |
| author | auth, creator, dev, developer | Information about this plugin's author | /autoecon author |
| bal | aebal, balance, bal | Get your current autoecon balance. | /bal |
| buy | purchase, get | Buy from the server market | /buy {item} [amount] [-stack] [-confirm] |
| deposit | give, ecogive | Deposit into a player's balance | /autoecon deposit {player} {amt} |
| dump |  | Dump worth from the market | /autoecon dump {amt} |
| pay | aepay, sendmoney, pay, transfer | Transfer a portion of your balance to another user | /pay {user} {amt} |
| pump |  | Pump worth into the market | /autoecon pump {amt} |
| reload | restart, load, refresh | Reload the plugin | /autoecon reload |
| save |  | Save the plugin's files | /autoecon save |
| sell | dump | Sell your items | /sell |
| setbalance | setbal | Set a player's balance. | /autoecon setbalance {player} {amt} |
| setexpression |  | Set the expression used for deriving the price of an item/reference | /autoecon setprice {reference} {price} |
| setprice |  | Set the price of an item using worth distribution | /autoecon setprice {reference} {price} |
| shop | market | Access the market shop | /shop [shop] [-preload:#] |
| trade |  | Trade with another player | /trade {player} [player...] |
| withdraw |  | Withdraw from a player's balance | /autoecon withdraw {player} {amt} |
| worth | price | Get the worth of an item | /worth item |
| order | request, placeorder | Customize an item purchase through a GUI | /order {item} [amt] [-stack] |
| cart | shoppingcart, scart, shoppingc, basket, checkout | The command for accessing a player's cart | /cart |
| checkout | pay, check | The command for checkout out all of the items in your cart | /cart checkout [item|slot] [amt] [-stack] |
| add | put, load, include | The command for checkout out all of the items in your cart | /cart add {item} [amt] |
| remove | negate, unload, exclude | The command for checkout out all of the items in your cart | /cart remove {item\|slot} [amt] [-stack] |
| clear | empty, burn, poof, dump | The command for clearing out one's shopping cart | /cart clear |


The *setprice* command does not create/destroy the difference in worth. It will trigger a dispersal of worth within the market to result in the specified item being worth the desired amount, which makes it suitable for changing the price of an item which depends on the price of other items. However, this also means that you cannot set the price of the item to one higher than the rest of the market can provide from its own worth. Consider pumping more worth into the economy first, if making large increases in item price.
Because the *setexpression* command modifies the raw expression used for the dermination of an item's worth, using this command causes a full rebuild of the market each time. Recommended only for quick fixes.


## Market  Types ##

By default, all user permissions direct to the main market. Consiquently, only the main market is generated. A new private market will be generated for every user who is explicitly granted permission to use a private market, and a new market group for each group that is defined in the *config.hmff*.

You might consider having your players use a private market if your server is one where players are disconnected from each other and their activities don't affect the supply and demand of items for other players.
Similarly, you might consider using market groups when you have groups of players whose activities affect the market economy of each other, but do not effect the market economy of players in other groups.

If your server calls for it, consider using a plugin like worldgaurd, which allows the server owner to grant and remove permissions to players within defined regions.
You may have one part of the world use one market group, and another part of the world use another. Players would then be able to walk/teleport/rank-up to an area with a different market from the place they came from.


## Events ##

com.gmail.justisroot.autoecon.api.**TransactionPreProcessEvent**

| Modifier and Type | Method | Description |
| ----------------- | ------ | ----------- |
| int | getAmount() | Get the amount of the item that is to be transacted ||
| java.math.BigDecimal | getChange()| Get the change in value to the item |
| static org.bukkit.event.HandlerList | getHandlerList() | Get the HandlerList of this event |
| org.bukkit.event.HandlerList | getHandlers() | Get the HandlerList of this event |
| org.bukkit.inventory.ItemStack | getItem() | Get an ItemStack representation of the item to be transacted |
| java.lang.String | getMarketID() | Get the ID of the market facilitating this transaction<br>Possible return values include "MAIN", "GROUP {groupname}" or "PRIVATE {player_uuid}" |
| java.math.BigDecimal | getOldWorth() | Get the previous sell value of the item |
| org.bukkit.entity.Player | getPlayer() | Get the player making this transaction |
| java.math.BigDecimal | getTotal() | Get the total value to be transacted |
| boolean | isCancelled() | Returns true if the transaction has been canceled, false if it has not been canceled |
| boolean | isSale() | Returns true if the transaction is a sale, false if it was a purchase |
| void | setCancelled(boolean cancel) | Set whether or not this transaction should be cancelled |
| void | setChange(java.math.BigDecimal newChange) | Set how much the price of the item will change as a result of this transaction. This will influence how much the price of every other item changes as well |
| void | setSale(boolean sale) | Set whether or not this was a sale or a purchase |
| void | setTotal(java.math.BigDecimal newTotal) | Set how much the transaction will have been for. Note that this is after the player's balance has been checked to ensure capacity. If you change the total of the transaction, ensure the transacting player has the balance necessary |


com.gmail.justisroot.autoecon.api.**TransactionCompleteEvent**

| Modifier and Type | Method | Description |
| ----------------- | ------ | ----------- |
| int | getAmount() | Get the amount of the item transacted |
| java.math.BigDecimal | getChange() | Get the chance in value to the item |
| static org.bukkit.event.HandlerList | getHandlerList() | Get the HandlerList of this event |
| org.bukkit.event.HandlerList | getHandlers() | Get the HandlerList of this event |
| org.bukkit.inventory.ItemStack | getItem() | Get an ItemStack representation of the transacted item |
| java.lang.String | getMarketID() | Get the ID of the market facilitating this transaction. Possible return values include "MAIN", "GROUP {groupname}" or "PRIVATE {player_uuid}" |
| java.math.BigDecimal | getNewWorth() | Get the new sell value of the item |
| java.math.BigDecimal | getOldWorth() | Get the previous sell value of the item |
| org.bukkit.entity.Player | getPlayer() | Get the player who made this transaction |
| java.math.BigDecimal | getTotal() | Get the total value transacted |
| boolean | isSale() | Returns true if the transaction was a sale, false if it was a purchase |
| boolean | isSuccess() | Returns true of the transaction was successful, false if something prevented the transaction from completing |

