# MottoBotto
MottoBotto is a configurable Discord bot with a penchant for mottos and words to live by. It sends approved nominated mottos to an Airtable base for further use.

## Default Usage TLDR

* Nominate somebody else's message as a potential motto with `!motto` in a reply to the message.
* Approve somebody's nomination of your message with an emoji reaction or a by joining an auto-approval role.
* View the [list of approved mottos and the leaderboard](https://dwrss.github.io/MottoBotto/).
* Change your leaderboard emoji with a direct message to MottoBotto of `!emoji <new-emoji>`.


## Interacting with MottoBotto

To nominate a motto for consideration, reply to the Discord message with one of MottoBotto's trigger phrases. The default triggers can be found in the [section below](#motto-nomination). MottoBotto will respond to your message with an emoji reaction indicating whether the nomination was accepted pending author approval, auto-accepted, rejected, invalid, or previously nominated. Mottos must be manually approved by moderators, and will then be available for display, along with a leaderboard for motto-makers, in [a simple web view](https://dwrss.github.io/MottoBotto/).

### Rules MottoBotto follows when accepting mottos

Mottos that are considered valid by MottoBotto are:

* at least 2 words in length,
* between 5 and 240 characters in length,
* are not purely punctuation, emoji, numeric, or url, and
* do not tag any Discord users.

Any suggested motto that doesn't conform to these rules will be rejected. 

MottoBotto will also reject any nomination that is a statement made by either yourself or MottoBotto.

### Rules humans should follow when suggesting mottos

1. The rules of the server should always be followed, and trump any other rules written here.
2. Aim to nominate mottos that have a useful sentiment rather than solely japes. The aim is to have a useful database of mottos from a variety of users that people can look over and get use from.
3. Do not abuse MottoBotto. Channels should not become spammed with nominations at the expense of the actual conversation that is happening. MottoBotto should aid the discussion, not hinder it.
4. Don’t fish for mottos. While there is a leaderboard, don’t try and cheat the system just to raise up the ranks. Instead, contribute to discussion naturally and helpfully and your reign will come.

### Nomination process

1. Nominate somebody's motto with one of the trigger phrases [listed below](#motto-nomination).
2. MottoBotto will respond to your nomination message with a "pending" emoji.
3. The author of the motto you nominated responds to your nomination message with an approval emoji.
4. MottoBotto will store the nominated motto in the leaderboard, and convert its "pending" emoji to a "success" emoji.

If you are happy for any message you send to be added to the leaderboard without your explicit approval, you may join the auto-approval role. In that situation, steps 2 and 3 above will be skipped.

### Adding or changing your emoji on the leaderboard

To add an emoji to your name on the leaderboard, change the emoji, or remove it, use the [change emoji trigger](#change-emoji) in a direct message to MottoBotto. The default is `!emoji`:

* `!emoji 🚀` will set the ​🚀​ emoji for your user.
* `!emoji` will clear any emoji for your user.

MottoBotto will respond with a reaction indicating a successful update or a problem with your request.

### Viewing the leaderboard

If a leaderboard is configured for MottoBotto, and MottoBotto has been added to the server with the permission to send messages, you can retrieve a link to it by using the `!link` command.

## Configuring MottoBotto

MottoBotto requires a `config.json` configuration file, with the following sections.

| Section                | Key             | Default Value                    | Required | Description                                                  |
| ---------------------- | --------------- | -------------------------------- | -------- | ------------------------------------------------------------ |
| `authentication`       | `discord`       | Empty string                     | Yes      | MottoBotto's DIscord bot token.                              |
|                        | `airtable_key`  | Empty string                     | Yes      | The API key for access to Airtable's API.                    |
|                        | `airtable_base` | Empty string                     | Yes      | The ID of the Airtable base to store the mottos.             |
| `channels`             | `exclude`       | Empty list                       | No       | A list of Discord channel names to ignore when reacting to triggers. |
|                        | `include`       | Empty list                       | No       | A list of Discord channels to specifically respond to triggers within. If specified, all other channels are ignored. |
| `reactions`            | `success`       | See below.                       | No       | The emoji to react to a successful nomination with.          |
|                        | `repeat`        | See below.                       | No       | The emoji to react to a nomination that has already been nominated with. |
|                        | `skynet`        | See below.                       | No       | The emoji to react to a nomination of a MottoBotto message with. |
|                        | `fishing`       | See below.                       | No       | The emoji to react to a nomination of the user's own message with. |
|                        | `invalid`       | See below.                       | No       | The emoji to react to invalid nominations with.              |
|                        | `invalid_emoji` | See below.                       | No       | The emoji to react to invalid emoji updates with.            |
|                        | `valid_emoji`   | See below.                       | No       | The emoji to react to successful emoji updates with.         |
| `should_reply`         | N/A             | `true`                           | No       | Whether to send message replies in response to nominations or not. If `false`, the only notifications users will receive are emoji reactions on their nomination message. |
| `rules`                | `matching`      | `^.{5,240}$`<br />`^(\S+\s+)\S+` | No       | A list of regular expressions to match against the nominated motto text that must all match for the motto to be accepted. The message is first stripped of leading and trailing whitespace before matching. * |
|                        | `excluding`     | `<@.*>`<br />`^[\d\W\s]*$`       | No       | A list of regular expressions to match against the nominated motto text, where any successful match will result in an invalid motto response. The message is first stripped of leading and trailing whitespace before matching. * |
| `triggers`             | `new_motto`     | `!motto$`                        | No       | A list of regular expressions to match against every incoming message in the relevant channels (see `channels` above) to recognise a new nomination. They are all prepended with `^` before matching, to ensure they match the start of the message. The message is first stripped of leading and trailing whitespace before matching. * |
|                        | `change_emoji`  | `!emoji`                         | No       | A list of regular expressions to match against every incoming direct message to recognise a request to change the user's emoji. They are all prepended with `^` before matching, to ensure they match the start of the message. The message is first stripped of leading and trailing whitespace before matching. * |
| `approval_reaction`    | N/A             | `mottoapproval`                  | No       | The name of the custom emoji used to approve the addition of one of your mottos. |
| `approval_opt_in_role` | N/A             | `Motto Opt In`                   | No       | The name of the role for a user to join to auto-approval all motto suggestions. |
| `leaderboard_link`     | N/A             | `None`                           | No       | A link to the motto leaderboard. If not configured, the `!link` DM will not be recognised. |

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
    "should_reply": false,
    "approval_reaction": "mottoapproval",
    "approval_opt_in_role": "Motto Opt In"
}
```

## MottoBotto Defaults
### Trigger Phrases

The trigger phrases detailed below are the defaults.  Any others for each trigger must be added as laid out [above](#configuring-mottobotto).

#### Motto Nomination

`!motto`

MottoBotto will always react with emoji, but can also be configured to react with a text message response. The defaults for both are as follows, although the emoji reactions can be changed in configuration:
* ⏳ MottoBotto is waiting for approval from the motto's author before adding the motto to the leaderboard. There is currently no corresponding text reply for this situation.
* 📥 MottoBotto added the nominated motto to the collection: "'Nominated-motto' will be considered!"
* ❓ MottoBotto does not know what you're responding to (i.e. the nominator has forgotten to reply to the motto they are nominating): "I see no motto!"
* ♻️ MottoBotto has previously added the nominated motto to the collection. There is currently no corresponding text reply for this situation.
* ❌ MottoBotto is not allowing itself to be nominated (i.e. the nominated message was written by MottoBotto): "Skynet prevention"
* 🎣 MottoBotto has rejected the motto for motto-fishing (i.e. the motto was written by the nominator): "Motto self-suggestions are forbidden"
* 🙅 MottoBotto has rejected the motto for violating at least one rule (e.g. the motto is shorter than two words, the motto @-mentions another user, etc.) There is currently no corresponding text reply for this situation.

#### Change Emoji

`!emoji`

This trigger phrase **must be sent as a direct message to MottoBotto**. If followed by an emoji (such as `!emoji 🚀`, it will set the user's emoji in the leaderboard to the specified emoji. If no emoji is specified, it will clear the emoji from the leaderboard for that user. It will only work for standard emoji, and not server-specific custom emoji.

MottoBotto will respond to the message with one of two reactions (the emoji for which can be changed in configuration). The defaults are as follows:

* ✅ The user's emoji was successfully updated.
* ⚠️ The emoji specified is not valid.

## Licensing

This code is copyright the contributors.

[MottoBotto is licensed under the Mozilla Public License 2.0](LICENSE)

### MottoBotto's Profile Image
Robot and Scroll Emoji Copyright 2020 Twitter, Inc and other contributors and licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)
