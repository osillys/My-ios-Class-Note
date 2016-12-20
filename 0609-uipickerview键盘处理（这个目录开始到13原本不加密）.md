## 二、点餐系统PickerView(熟悉) 2014年9月17日星期三 上午9:16

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

## KVC
- KVC赋值流程

```objc
//遍历字典里面所有的key

//  key：name
//  就去模型中查找有没有setName:,直接调用这个对象setName:赋值
//  假如没有找到setName:。就会去模型中查找有没有_name属性，_name = value
//  假如没有找到_name,还会去模型中查找name属性
//  最终没有找到，就会直接报错。
```

- KVC实现方法：

```objc
+ (instancetype)flageWithDict:(NSDictionary *)dict
{
    XMGFlag *flag = [[self alloc] init];
    
    // 利用KVC字典转模型
/／ [flag setValuesForKeysWithDictionary:dict];
    
    //自己实现KeyValueCoding的代码
    [dict enumerateKeysAndObjectsUsingBlock:^(NSString *key, id obj, BOOL *stop) {
        NSString *funcName = [NSString stringWithFormat:@"set%@",key.capitalizedString];
        
        if ([flag respondsToSelector:@selector(funcName)]) {           
            [flag setValue:obj forKeyPath:key];           
        }
    }];
    
    return flag;
}
```

## 自定义键盘（将键盘输入改为UIPickview，且只能用pickview修改输入值）

```objc
#pragma mark - UITextFieldDelegate
// 是否允许开始编辑
//- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField
//{
//    return NO;
//}

// 是否允许结束编辑
//- (BOOL)textFieldShouldEndEditing:(UITextField *)textField
//{
//    return NO;
//}


// 是否允许用户输入文字
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    return NO;
}

// 文本框开始编辑的时候调用
- (void)textFieldDidBeginEditing:(UITextField *)textField
{
    // 给生日文本框赋值
    [self dateChange:_datePicker];
}
- (void)viewDidLoad {
    [super viewDidLoad];

    // 自定义生日键盘
    [self setUpBirthdayKeyboard];
}

// 自定义生日键盘
- (void)setUpBirthdayKeyboard
{
    // 创建UIDatePicker
    // 注意：UIDatePicker有默认的尺寸，可以不用设置frame
    UIDatePicker *picker = [[UIDatePicker alloc] init];
    
    _datePicker = picker;
    
    // 设置地区 zh:中国
    picker.locale = [NSLocale localeWithLocaleIdentifier:@"zh"];
    
    // 设置日期的模式
    picker.datePickerMode = UIDatePickerModeDate;
    
    // 监听UIDatePicker的滚动
    [picker addTarget:self action:@selector(dateChange:) forControlEvents:UIControlEventValueChanged];
    
    
    _birthdayField.inputView = picker;
}

// 当UIDatePicker滚动的时候调用
// 给生日文本框赋值
- (void)dateChange:(UIDatePicker *)datePicker
{
    NSLog(@"%@",datePicker.date);
    // 日期转换字符串
    
    NSDateFormatter *fmt = [[NSDateFormatter alloc] init];

    fmt.dateFormat = @"yyyy-MM-dd";
    
    NSString *dateStr = [fmt stringFromDate:datePicker.date];
    
    _birthdayField.text = dateStr;
}
```

- 借由NSDateFormatter将UIDatePickView的NSDate属性改为NSString：

```objc
NSDateFormatter *fmt = [[NSDateFormatter alloc] init];

fmt.dateFormat = @"yyyy-MM-dd";
NSString *dateStr = [fmt stringFromDate:datePicker.date];
```