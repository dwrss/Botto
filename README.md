# MottoBotto
MottoBotto is a configurable Discord bot with a penchant for mottos and words to live by. It sends nominated mottos to an AirTable base for further use.

## Interacting with MottoBotto

To nominate a motto for consideration, reply to the Discord message with one of MottoBotto's trigger phrases. The default triggers can be found in the [section below](#mottobottos-defaults). MottoBotto will respond to your message with an emoji reaction indicating whether the nomination was accepted, rejected, invalid, or previously nominated.

### Rules MottoBotto follows when accepting mottos

Mottos that are considered valid by MottoBotto are:

* at least two words in length,
* between five and 240 characters in length,
* are not purely punctuation, emoji, or numeric, and
* do not tag any Discord users.

Any suggested motto that doesn't conform to these rules will be rejected. 

MottoBotto will also reject any nomination that is a statement made by either yourself or MottoBotto.

### Rules humans should follow when suggesting mottos
1. The rules of the server should always be followed, and trump any other rules written here.
2. Aim to nominate mottos that have a useful sentiment rather than solely japes. The aim is to have a useful database of mottos from a variety of users that people can look over and get use from.
3. Do not abuse MottoBotto. Channels should not become spammed with nominations at the expense of the actual conversation that is happening. MottoBotto should aid the discussion, not hinder it.
4. Don’t fish for mottos. While there is a leaderboard, don’t try and cheat the system just to raise up the ranks. Instead, contribute to discussion naturally and helpfully and your reign will come.

## Configuring MottoBotto

MottoBotto requires a `config.json` configuration file, with the following sections.

| Section          | Key             | Default Value                    | Required | Description                                                  |
| ---------------- | --------------- | -------------------------------- | -------- | ------------------------------------------------------------ |
| `authentication` | `discord`       | None                             | Yes      | MottoBotto's DIscord bot token.                              |
|                  | `airtable_key`  | None                             | Yes      | The API key for access to Airtable's API.                    |
|                  | `airtable_base` | None                             | Yes      | The ID of the Airtable base to store the mottos.             |
| `channels`       | `exclude`       | Empty list                       | No       | A list of Discord channel names to ignore when reacting to triggers. |
|                  | `include`       | Empty list                       | No       | A list of Discord channels to specifically respond to triggers within. If specified, all other channels are ignored. |
| `reactions`      | `success`       | See below.                       | No       | The emoji to react to a successful nomination with.          |
|                  | `repeat`        | See below.                       | No       | The emoji to react to a nomination that has already been nominated with. |
|                  | `skynet`        | See below.                       | No       | The emoji to react to a nomination of a MottoBotto message with. |
|                  | `fishing`       | See below.                       | No       | The emoji to react to a nomination of the user's own message with. |
|                  | `invalid`       | See below.                       | No       | The emoji to react to invalid nominations with.              |
|                  | `invalid_emoji` | See below.                       | No       | The emoji to react to invalid emoji updates with.            |
|                  | `valid_emoji`   | See below.                       | No       | The emoji to react to successful emoji updates with.         |
| `should_reply`   | N/A             | `true`                           | No       | Whether to send message replies in response to nomations or not. If `false`, the only notifications users will receive are emoji reactions on their nomination message. |
| `rules`          | `matching`      | `^.{5,240}$`<br />`^(\S+\s+)\S+` | No       | A list of regular expressions to match against the nominated motto text that must all match for the motto to accepted. The message is first stripped of leading and trailing whitespace before matching. * |
|                  | `excluding`     | `<@.*>`<br />`^[\d\W\s]*$`       | No       | A list of regular expressions to match against the nominated motto text, where any successful match will result in an invalid motto response. The message is first stripped of leading and trailing whitespace before matching. * |
| `triggers`       | `new_motto`     | `!motto$`                        | No       | A list of regular expressions to match against every incoming message in the relevant channels (see `channels` above) to recognise a new nomination. They are all prepended with `^` before matching, to ensure they match the start of the message. The message is first stripped of leading and trailing whitespace before matching. |
|                  | `change_emoji`  | `!emoji`                         | No       | A list of regular expressions to match against every incoming direct message to recognise a request to change the user's emoji. They are all prepended with `^` before matching, to ensure they match the start of the message. The message is first stripped of leading and trailing whitespace before matching. |

\*Note: Regular expressions used for motto nomination rule matching are matched with case sensitivity, and must include the `^` and `$` if you wish to match against the entire message string. Those used for trigger phrases are matched without regard for case.

### Example configuration

The following is a full example `config.json`.

```json
{
    "authentication": {
        "discord": "REDACTED",
        "airtable_key": "REDACTED",
        "airtable_base": "REDACTED"
    },
    "channels": {
        "exclude": [
            "ignore-this-channel"
        ]
    },
    "rules": {
        "excluding": [
          "^HELLO!$"
        ]
    },
    "reactions": {
        "success": "📥",
        "repeat": "♻️",
        "unknown": "❓",
        "skynet": "❌",
        "fishing": "🎣"
    },
    "triggers": {
        "new_motto": [
            "!motto$",
	          "Accurate[.,!] New motto\\?"
        ]
    },
    "should_reply": false
}
```

## MottoBotto's Defaults
MottoBotto has one recognizable command by default (all others must be added as laid out [above](#configuring-mottobotto)):
* !motto

Depending on whether MottoBotto is configured to react with words, or with emoji, the defaults are as follows:
* MottoBotto added the nominated motto to the collection: 📥 or "'Nominated-motto' will be considered!"
* MottoBotto does not know what you're responding to (i.e. the nominator has forgotten to reply to the motto they are nominating):❓or "I see no motto!"
* MottoBotto has previously added the nominated motto to the collection: ♻️ or ""
* MottoBotto is not allowing itself to be nominated (i.e. the nominated message was written by MottoBotto): ❌ or "Skynet prevention"
* MottoBotto has rejected the motto for motto-fishing (i.e. the motto was written by the nominator): 🎣 or "Motto self-suggestions are forbidden"
* MottoBotto has rejected the motto for violating at least one rule (e.g. the motto is shorter than two words, the motto has a url in it, etc.): 🙅 or ""