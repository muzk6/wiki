# etc

## 复制内容到剪贴板

```
function CopyText(id) {
	try{
		var targetText = document.getElementById(id);
		targetText.focus();
		targetText.select();
		var clipeText = targetText.createTextRange();
		clipeText.execCommand("Copy");
	}catch(e){}
}
```