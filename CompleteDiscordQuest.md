## Complete Recent Discord Quest
> [!NOTE]
> This no longer works in browser! If you absolutely need to use browser instead of desktop app, use an extension to add the string `Electron/` anywhere in your user-agent.

> [!NOTE]
> This no longer works if you're alone in vc! Somebody else has to join you!
>

How to use this script:
1. Accept the quest under User Settings -> Gift Inventory
2. Join a vc
3. **Join the same vc on an alt**
4. Stream any window (can be notepad or something)
5. Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd> to open DevTools
6. Go to the `Console` tab
7. Paste the following code and hit enter:
```js
let wpRequire;
window.webpackChunkdiscord_app.push([[ Math.random() ], {}, (req) => { wpRequire = req; }]);

let api = Object.values(wpRequire.c).find(x => x?.exports?.getAPIBaseURL).exports.HTTP;
let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getStreamerActiveStreamMetadata).exports.default;
let VoiceStateStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getCurrentClientVoiceChannelId).exports.default;
let QuestsStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getQuest).exports.default;
let encodeStreamKey = Object.values(wpRequire.c).find(x => x?.exports?.encodeStreamKey).exports.encodeStreamKey;
let sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

let quest = [...QuestsStore.quests.values()].find(x => x.userStatus?.enrolledAt && !x.userStatus?.completedAt && new Date(x.config.expiresAt).getTime() > Date.now())
let streamData = ApplicationStreamingStore.getCurrentUserActiveStream()
let isApp = navigator.userAgent.includes("Electron/")
let isAloneInVC = streamData && Object.keys(VoiceStateStore.getVoiceStatesForChannel(streamData.channelId)).length === 1
if(!isApp) {
	console.log("This no longer works in browser. Use the desktop app!")
} else if(!quest) {
	console.log("You don't have any uncompleted quests!")
} else if(!streamData) {
	console.log("You haven't started a stream!")
} else if(isAloneInVC) {
	console.log("You need to join the vc on 1 other account!")
} else {
	let streamId = encodeStreamKey(streamData)
	let secondsNeeded = quest.config.streamDurationRequirementMinutes * 60
	let heartbeat = async function() {
		console.log("Completing quest", quest.config.messages.gameTitle, "-", quest.config.messages.questName)
		while(true) {
			let res = await api.post({url: `/quests/${quest.id}/heartbeat`, body: {stream_key: streamId}, headers: {"X-Discord-Resource-Optimization-Level": "1"}})
			let progress = res.body.stream_progress_seconds
			
			console.log(`Quest progress: ${progress}/${secondsNeeded}`)
			
			if(progress >= secondsNeeded) break;
			await sleep(30 * 1000)
		}
		
		console.log("Quest completed!")
	}
	heartbeat()
}
```
7. Keep the stream running for 15 minutes
8. You can now claim the reward in User Settings -> Gift Inventory!

You can track the progress by looking at the `Quest progress:` prints in the Console tab, or by reopening the Gift Inventory tab in settings. The progress should update every 30s.

## FAQ

**Q: Ctrl + Shift + I doesn't work**

A: Either download the [ptb client](https://discord.com/api/downloads/distributions/app/installers/latest?channel=ptb&platform=win&arch=x64), or use [this](https://www.reddit.com/r/discordapp/comments/sc61n3/comment/hu4fw5x/) to enable DevTools on stable


**Q: I get an error saying "Unauthorized"**

A: Discord has patched the script from working in browsers. Use the desktop app, or alternatively find some extension which lets you change your User-Agent and append the string `Electron/` anywhere in it.

They have also started checking how many people are in the vc, so make sure you join it on at least 1 other account.


**Q: I get a different error**

A: Make sure you've started streaming *before* running the script. Also make sure you're copy/pasting it correctly.