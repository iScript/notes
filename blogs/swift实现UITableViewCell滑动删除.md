title: swift实现UITableViewCell滑动删除   
date: 2015-11-06 14:00:39  
tags:  
	- swift  
	- iOS
	 

---

###  UITableViewCell滑动删除  
实现Cell的滑动删除，只需重写UITableViewDelegate代理中的如下2个方法。  

```swift  

// 进入编辑模式，按下编辑按钮后执行  
func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
    //删除数组中的数据
    self.dataArray.removeAtIndex(indexPath.row);  
	//删除单元格
	self.tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: UITableViewRowAnimation.Top);
}
    
//修改文字  
func tableView(tableView: UITableView, titleForDeleteConfirmationButtonForRowAtIndexPath indexPath: NSIndexPath) -> String? {
    return "啊~不要";
}    
```  
 
 ![swift](https://dn-yuankeqiang.qbox.me/uitableviewcell11.gif)  
 
 
 ### UITableview 自定义Action  
 
上面只实现了UITableViewCell的单个编辑Action，若要自定义Action，可使用如下的代理方法。  

```swift  

func tableView(tableView: UITableView, editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [UITableViewRowAction]? {
    let action1 = UITableViewRowAction(style: UITableViewRowActionStyle.Default, title: "删除") { (action , index) -> Void in
    	print("点击了删除");
    }
    let action2 = UITableViewRowAction(style: UITableViewRowActionStyle.Normal, title: "取消关注") { (action , index) -> Void in
        print("点击了取消关注");
    }
    return [action1,action2];
}  
    
```  

![swift](https://dn-yuankeqiang.qbox.me/uitableviewcell2.gif)  


 
 


