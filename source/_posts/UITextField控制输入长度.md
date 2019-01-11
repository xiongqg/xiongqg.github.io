---
title: UITextField控制输入长度
date: 2018-09-04 19:50:10
tags: 随笔
description: UITextField控制输入长度

---

~~~
#define kMaxLength 8

self.accountTextField=[[UITextField alloc]initWithFrame:CGRectMake(26, CGRectGetMaxY(self.titleLabel.frame)+6, bgView.frame.size.width-52, 38)];
[self.accountTextField setFont:[UIFont systemFontOfSize:14]];
[self.accountTextField setBorderStyle:UITextBorderStyleRoundedRect];
[self.accountTextField setPlaceholder:@"请输入昵称"];
~~~
<!--more-->
~~~
[self.accountTextField addTarget:self action:@selector(editChange:) forControlEvents:UIControlEventEditingChanged];
[self.accountTextField setClearButtonMode:UITextFieldViewModeAlways];
[bgView addSubview:self.accountTextField];

- (void)editChange:(UITextField*)textfield {
	NSString *toBeString = textfield.text;
	NSString *lang = [[UIApplication sharedApplication]textInputMode].primaryLanguage; // 键盘输入模
	if ([lang isEqualToString:@"zh-Hans"]) {
		 // 简体中文输入，包括简体拼音，健体五笔，简体手写
		UITextRange *selectedRange = [textfield markedTextRange];
		//获取高亮部分
		UITextPosition *position = [textfield positionFromPosition:selectedRange.start offset:0];
		// 没有高亮选择的字，则对已输入的文字进行字数统计和限制
		if (!position) {
			if (toBeString.length > kMaxLength)
			{
				NSRange rangeIndex = [toBeString rangeOfComposedCharacterSequenceAtIndex:kMaxLength];
				if (rangeIndex.length == 1)
				{
					textfield.text = [toBeString substringToIndex:kMaxLength];
				}
				else
				{
					NSRange rangeRange = [toBeString rangeOfComposedCharacterSequencesForRange:NSMakeRange(0, kMaxLength)];
					textfield.text = [toBeString substringWithRange:rangeRange];
				}
			}
		}
	}
// 中文输入法以外的直接对其统计限制即可，不考虑其他语种情况
	else{
		if (toBeString.length > kMaxLength)
		{
			NSRange rangeIndex = [toBeString rangeOfComposedCharacterSequenceAtIndex:kMaxLength];
			if (rangeIndex.length == 1)
			{
				textfield.text = [toBeString substringToIndex:kMaxLength];
			}
			else
			{
				NSRange rangeRange = [toBeString rangeOfComposedCharacterSequencesForRange:NSMakeRange(0,kMaxLength)];
				textfield.text = [toBeString substringWithRange:rangeRange];
			}
		}
	}

}
~~~
