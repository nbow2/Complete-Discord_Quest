## Complete Discord Mokoko Quest
How to use this script:
1. Accept the quest under User Settings -> Gift Inventory
2. Join a vc
3. Stream any window (can be notepad or something)
4. Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd> to open DevTools
5. Go to the `Console` tab
6. Paste the following code and hit enter:
```js
let wpRequire;
window.webpackChunkdiscord_app.push([[ Math.random() ], {}, (req) => { wpRequire = req; }]);

let api = Object.values(wpRequire.c).find(x => x?.exports?.getAPIBaseURL).exports.HTTP;
let ChannelStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getSortedPrivateChannels).exports.default;
let SelectedChannelStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getVoiceChannelId).exports.default;
let UserStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getCurrentUser).exports.default;
let sleep = ms => new Promise(resolve => setTimeout(resolve, ms))

let vcId = SelectedChannelStore.getVoiceChannelId()
let streamId = `guild:${ChannelStore.getChannel(vcId).guild_id}:${vcId}:${UserStore.getCurrentUser().id}`
let heartbeat = async function() {
	while(true) {
		let res = await api.post({url: "/quests/1227395355193118750/heartbeat", body: {stream_key:streamId}})
		let progress = res.body.stream_progress_seconds
		
		if(progress >= 900) break;
		await sleep(30 * 1000)
	}
}
heartbeat()
```
7. Keep the stream running for 15 minutes
8. You can now claim the decoration in User Settings -> Gift Inventory!