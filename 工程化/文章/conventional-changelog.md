[onventional-changelog](https://github.com/conventional-changelog) 是一款可以根据项目的`commit` 和 `metadata`信息自动生成 `changelogs` 和 `release notes`的系列工具，并且在辅助 [standard-version](https://github.com/conventional-changelog/standard-version) 工具的情况下，可以自动帮你完成生成`version`、打`tag`, 生成`CHANGELOG`等系列过程。

```
{
	  "scripts": {
	    "version": "conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md"
	  }
}
```

![[Pasted image 20220816154216.png]]