# Discord (Apple Silicon)

This is a guide that is based off of the work of Arthur Lockman (@arthurlockman), the original guide can be found at https://rthr.me/2021/03/discord-native-apple-silicon/. At the time off writing, their exact instructions don't work as far as I'm aware, which is the motivation for the writing of this guide.

This build requires the use of nativefier, a tool that can wrap webapps in an electron-wrapper that runs natively on apple silicon chips. This can be installed with the following command. Though you will need node.js installed to run it, which can be installed with one of the following links found at https://nodejs.org/en/.

```
npm install -g nativefier
```

In addition you will need to find an icon to use with the build. This can be acquired at https://macosicons.com

Once nativefier and the icon are downloaded, name the icon ```discord.icns``` and place it in a folder of your choice. In addition you will want to make a script called discord-platform-osx.js and place it in the same folder, this script makes the top bar feel and act more like the official client.

The script will have the following contents.

```
new MutationObserver(function (mutationList, observer) {
  mutationList.forEach((mutation) => {
    if (mutation.attributeName == 'class' &&
        mutation.target.classList.contains('platform-web')) {
      mutation.target.classList.remove('platform-web');
      mutation.target.classList.add('platform-osx');
    }
  });
}).observe(document.documentElement, {attributes: true,
                                      characterData: false,
                                      childList: false});
```

Finally, open a terminal window and run the following command in the directory containing the files.

```
nativefier --enable-es3-apis --icon discord.icns --browserwindow-options '{ "fullscreenable": "true", "simpleFullscreen": "false" }' --darwin-dark-mode-support --background-color '#36393f' --inject discord-platform-osx.js --title-bar-style hiddenInset -a arm64 https://discord.com/app
```

It will create an application in the newly created folder ./Discord-darwin-arm64, you will want to move the folder inside of the applications folder on your mac.

Alternatively, I have released a prebuilt application that you are able to place directly into your applications folder.
