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
let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getStreamerActiveStreamMetadata).exports.default;
let encodeStreamKey = Object.values(wpRequire.c).find(x => x?.exports?.encodeStreamKey).exports.encodeStreamKey;
let sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

let streamId = encodeStreamKey(ApplicationStreamingStore.getCurrentUserActiveStream())
let heartbeat = async function() {
	while(true) {
		let res = await api.post({url: "/quests/1227395355193118750/heartbeat", body: {stream_key: streamId}})
		let progress = res.body.stream_progress_seconds
		
		console.log(`Quest progress: ${progress}/900`)
		
		if(progress >= 900) break;
		await sleep(30 * 1000)
	}
	
	console.log("Quest completed!")
}
heartbeat()
```
7. Keep the stream running for 15 minutes
8. You can now claim the decoration in User Settings -> Gift Inventory!

You can track the progress by looking at the `Quest progress:` prints in the Console tab, or by reopening the Gift Inventory tab in settings. The progress should update every 30s.

You do NOT need anybody watching your stream for this to work. You can be alone in vc just fine.