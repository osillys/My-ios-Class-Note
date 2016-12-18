# 二、点餐系统PickerView(熟悉) 2014年9月17日星期三 上午9:16

- 1.搭建界面1> 注意点:PickerView的高度不能改，默认162，PickerView里面每行的高度 可以改，不要弄混淆了。
- 2.pickerView显示数据
 - 1> 如何使用PickerView展示数据? 进入PickerView头文件，有数据源和代理，联想到UITableView,模仿 UITableView的用法。
 - 2> 让控制器作为PickerView的数据源，控制器遵守PickerView的数据源方法
   - 2.1>两种方式:1.拖线 2.代码 
   - 2.2>系统自带的控件，数据源和代理属性不需要IBOutlet，也能拖 线。自己的属性，想要拖线，必须写IBOutlet。
 - 3> PickerView的数据源方法
   - 1> numberOfComponentsInPickerView: 返回多少列
   - 2> pickerView:numberOfRowsInComponent: 返回第component列有多少 行
   - 3> 和UITableView的区别，每一行长什么样，是由PickerView的代理决 定的。
   - 4> 注意:如果没有返回每一行长什么样子，每行就会显示?，看见?,就 知道没有实现每一行长什么样子的方法。
 - 4> PickerView的代理方法
   - 1> 返回第component列第row行长什么样。          
   
```objc
第component列第row行的展示标题- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component第component列第row行带属性的标题- (NSAttributedString *)pickerView:(UIPickerView *)pickerView attributedTitleForRow:(NSInteger)row forComponent:(NSInteger) component第component列第row行展示的视图- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view;

```