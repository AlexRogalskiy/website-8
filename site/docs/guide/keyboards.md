---
prev: ./errors.md
next: ./inline-queries.md
---

# Keyboards and Inline Keyboards

Your bot may send a number of buttons, either to [replace the user's keyboard](#keyboards), or to be [displayed underneath a message](#inline-keyboards).

## Keyboards

> Revisit the keyboard section in the [Introduction for Developers](https://core.telegram.org/bots#keyboards) written by the Telegram team.

grammY has a simple and intuitive way to build up the reply keyboards that your bot can use to replace the user's keyboard.
It provides a class called `Keyboard` for this.

Once a user clicks a text button, your bot will receive the sent text as a plain text message.
Remember that you can listed for text message via `bot.on('message:text')`.

### Building a keyboard

Here are three examples how to build a keyboard with `text` buttons.

You can also request the phone number with `requestContact`, the location with `requestLocation`, and a poll with `requestPoll`.

#### Example 1

Three buttons in one column can be built like this:

##### Code

```ts
const keyboard = new Keyboard()
  .text("Yes, they certainly are").row()
  .text("I'm not quite sure").row()
  .text("No. 😈");
```

##### Result

![Example 1](https://core.telegram.org/file/811140184/1/5YJxx-rostA/ad3f74094485fb97bd)

#### Example 2

A calculator keyboard can be built like this:

##### Code

```ts
const keyboard = new Keyboard()
  .text("7").text("8").text("9").text("*").row()
  .text("4").text("5").text("6").text("/").row()
  .text("1").text("2").text("3").text("-").row()
  .text("0").text(".").text("=").text("+");
```

##### Result

![Example 2](https://core.telegram.org/file/811140880/1/jS-YSVkDCNQ/b397dfcefc6da0dc70)

#### Example 3

Three buttons in one column can be built like this:

##### Code

```ts
const keyboard = new Keyboard()
    .text("A").text("B").row()
    .text("C").text("D");
```

##### Result

![Example 3](https://core.telegram.org/file/811140733/2/KoysqJKQ_kI/a1ee46a377796c3961)

### Sending a keyboard

You can send a keyboard directly along a message, no matter whether you use `bot.api.sendMessage`, `ctx.api.sendMessage`, or `ctx.reply`:

```ts
// Send keyboard with message:
await ctx.reply(text, {
  // or bot.api.sendMessage
  reply_markup: keyboard,
});
```

If you want to specify further options with your message, you may need to create your own `reply_markup` object.
In that case, you have to use `keyboard.build()` when passing it to your custom object.

```ts
await ctx.reply(textWithHtml, {
  reply_markup: {
    // have to create custom `reply_markup` object
    parse_mode: "HTML", // in order to pass value for `parse_mode`
    keyboard: keyboard.build(), // note the `build` call
  },
});
```

Naturally, all other methods that send messages other than text messages support the same options, as specified by the [Telegram Bot API Reference](https://core.telegram.org/bots/api).

## Inline Keyboards

> Revisit the inline keyboard section in the [Introduction for Developers](https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating) written by the Telegram team.

grammY has a simple and intuitive way to build up the inline keyboards that your bot can send along with a message.
It provides a class called `InlineKeyboard` for this.

Every `text` button has a string as callback data attached.
If you don't attach callback data, grammY will use the button's text as data.

Once a user clicks a text button, your bot will receive an update the corresponding button's callback data.
You can listed for callback data via `bot.callbackQuery()`, and for game queries via `bot.gameQuery()`.

> Both `switchInline` and `switchInlineCurrent` buttons start inline queries.
> Check out the section about [Inline queries](./inline-queries.md) for more information about how they work.

### Building an inline keyboard

Here are three examples how to build an inline keyboard with `text` buttons.

You can also use other methods like `url` to open a URL, and more options as listed in the [grammY API Reference for InlineKeyboard](https://doc.deno.land/https/deno.land/x/grammy/mod.ts#InlineKeyboard) as well as the [Telegram Bot API Reference](https://core.telegram.org/bots/api#inlinekeyboardbutton).

#### Example 1

Buttons for a pagination navigation can be built like this:

##### Code

```ts
const inlineKeyboard = new InlineKeyboard()
  .text("« 1", "first")
  .text("‹ 3", "prev")
  .text("· 4 ·", "stay")
  .text("5 ›", "next")
  .text("31 »", "last");
```

##### Result

![Example 1](https://core.telegram.org/file/811140217/1/NkRCCLeQZVc/17a804837802700ea4)

#### Example 2

An inline keyboard with share button can be built like this:

##### Code

```ts
const inlineKeyboard = new InlineKeyboard()
  .text("Get random music", "random")
  .switchInline("Send music to friends");
```

##### Result

![Example 2](https://core.telegram.org/file/811140659/1/RRJyulbtLBY/ea6163411c7eb4f4dc)

#### Example 3

URL buttons can be built like this:

##### Code

```ts
const inlineKeyboard = new InlineKeyboard().url(
  "Read on TechCrunch",
  "https://techcrunch.com/2016/04/11/this-is-the-htc-10/"
);
```

##### Result

![Example 3](https://core.telegram.org/file/811140999/1/2JSoUVlWKa0/4fad2e2743dc8eda04)

### Sending an inline keyboard

You can send an inline keyboard directly along a message, no matter whether you use `bot.api.sendMessage`, `ctx.api.sendMessage`, or `ctx.reply`:

```ts
// Send inline keyboard with message:
await ctx.reply(text, {
  // or bot.api.sendMessage
  reply_markup: inlineKeyboard,
});
```

If you want to specify further options with your message, you may need to create your own `reply_markup` object.
In that case, you have to use `inlineKeyboard.build()` when passing it to your custom object.

```ts
await ctx.reply(textWithHtml, {
  reply_markup: {
    // have to create custom `reply_markup` object
    parse_mode: "HTML", // in order to pass value for `parse_mode`
    inline_keyboard: inlineKeyboard.build(), // note the `build` call
  },
});
```

Naturally, all other methods that send messages other than text messages support the same options, as specified by the [Telegram Bot API Reference](https://core.telegram.org/bots/api).
