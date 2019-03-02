---
layout:     post
title:      ReactiveCocoa ����
subtitle:   ����ʽ��̿�� ReactiveCocoa ����
date:       2017-01-06
author:     BY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS
    - ReactiveCocoa
    - ����ʽ���
    - ��Դ���
---
# ǰ��

>��[��ƪ����](http://qiubaiying.github.io/2016/12/26/ReactiveCocoa-����/)�н�����**ReactiveCocoa**�Ļ���֪ʶ,�������������������**ReactiveCocoa**������**MVVM**�е��÷���


![ReactiveCocoa����˼ά��ͼ](https://ww3.sinaimg.cn/large/006y8lVagw1fbgye3re5xj30je0iomz8.jpg)
# ����������������


#### ������֪

���е��źţ�RACSignal�������Խ��в���������Ϊ���в���������������RACStream.h�У����ֻҪ�̳�RACStream�����˲�����������
#### ����˼��

���õ���Hook�����ӣ�˼�룬Hook��һ�����ڸı�API(Ӧ�ó����̽ӿڣ�����)ִ�н���ļ���.

Hook�ô����ػ�API���õļ�����

�й�Hook��֪ʶ���Կ��ҵ���ƪ����[��Objective-C Runtime ��һЩ����ʹ�á�](http://www.jianshu.com/p/ff114e69cc0a)�е� *���������ʵ�ַ���* һ��,

Hookԭ����ÿ�ε���һ��API���ؽ��֮ǰ����ִ�����Լ��ķ������ı����������

#### ��������

#### **bind**���󶨣�- ReactiveCocoa���ķ���

**ReactiveCocoa** �����ĺ��ķ����� **bind**���󶨣�,����Ҳ��RAC�к��Ŀ�����ʽ��֮ǰ�Ŀ�����ʽ�Ǹ�ֵ������RAC������Ӧ�ð����ķ��ڰ󶨣�Ҳ���ǿ����ڴ���һ�������ʱ�򣬾Ͱ󶨺��Ժ���Ҫ�������飬�����ǵȸ�ֵ֮����ȥ�����顣

���磬������չʾ���ؼ��ϣ�֮ǰ������д�ؼ��� `setModel` ��������RAC�Ϳ�����һ��ʼ�����ؼ���ʱ�򣬾Ͱ󶨺����ݡ�

- **����**

	RAC�ײ㶼�ǵ���**bind**�� �ڿ����к���ֱ��ʹ�� **bind** ������**bind**����RAC�еĵײ㷽��������ֻ��Ҫ���÷�װ�õķ�����**bind**�����˽⼴��.

- **bind����ʹ�ò���**
     1. ����һ������ֵ `RACStreamBindBlock` �� block��
     2. ����һ�� `RACStreamBindBlock` ���͵� `bindBlock`��Ϊblock�ķ���ֵ��
     3. ����һ�����ؽ�����źţ���Ϊ `bindBlock` �ķ���ֵ��
     
     ע�⣺��bindBlock�����źŽ���Ĵ���
- 	**bind��������**
	
	**RACStreamBindBlock**:
`typedef RACStream * (^RACStreamBindBlock)(id value, BOOL *stop);`

     `����һ(value)`:��ʾ���յ��źŵ�ԭʼֵ����û������
     
     `������(*stop)`:�������ư�Block�����*stop = yes,��ô�ͻ�����󶨡�
     
     `����ֵ`���źţ����ô�����ͨ������źŷ��س�ȥ��һ��ʹ�� `RACReturnSignal`,��Ҫ�ֶ�����ͷ�ļ�`RACReturnSignal.h`

- **ʹ��**

	����������ı�������ݣ�������ÿ����������ʱ�򣬶����ı��������ƴ��һ�����֡��������

	- ʹ�÷�װ�õķ������ڷ��ؽ����ƴ�ӡ�

		```
		[_textField.rac_textSignal subscribeNext:^(id x) {
		
			// �ڷ��ؽ����ƴ�� �����
			NSLog(@"���:%@",x);
		
		}];
		```


	- ��ʽ��:��ʹ��RAC�� `bind` �����������ڷ��ؽ��ǰ��ƴ�ӡ�
	  
		������Ҫ�ֶ�����`#import <ReactiveCocoa/RACReturnSignal.h>`������ʹ��`RACReturnSignal`

		```	
		[[_textField.rac_textSignal bind:^RACStreamBindBlock{
		   // ʲôʱ�����:
		   // block����:��ʾ����һ���ź�.
		
		   return ^RACStream *(id value, BOOL *stop){
		
		       // ʲôʱ�����block:���ź����µ�ֵ�������ͻ��������block��
		
		       // block����:������ֵ�Ĵ���
		
		       // ���ô����ڷ��ؽ��ǰ��ƴ�� ���:
		       return [RACReturnSignal return:[NSString stringWithFormat:@"���:%@",value]];
		   };
		
		}] subscribeNext:^(id x) {
		
		   NSLog(@"%@",x);
		
		}];

		```

- **�ײ�ʵ��**
     1. Դ�źŵ���bind,�����´���һ�����źš�
     2. �����źű����ģ��ͻ���ð��ź��е� `didSubscribe` ������һ�� `bindingBlock` ��
     3. ��Դ�ź������ݷ������ͻ�����ݴ��ݵ� `bindingBlock` ��������`bindingBlock(value,stop)`
     4. ����`bindingBlock(value,stop)`���᷵��һ�����ݴ�����ɵ��ź�`RACReturnSignal`��
     5. ����`RACReturnSignal`���ͻ��õ����źŵĶ����ߣ��Ѵ�����ɵ��ź����ݷ��ͳ�����
    
     ע��:��ͬ�����ߣ����治ͬ��nextBlock����Դ���ʱ��һ��Ҫ��������������ĸ���

#### ӳ��

ӳ����Ҫ������������ʵ�֣�**flattenMap**,**Map**,���ڰ�Դ�ź�����ӳ����µ����ݡ�

###### flattenMap

- **����**

	��Դ�źŵ�����ӳ���һ���µ��źţ��źſ�������������

- **ʹ�ò���**

     1. ����һ��block��block�����Ƿ���ֵ`RACStream`������value
     2. ����value����Դ�źŵ����ݣ��õ�Դ�źŵ�����������
     3. ��װ��`RACReturnSignal`�źţ����س�ȥ��



- **ʹ��**

	�����ı�������ݸı䣬�ѽṹ����ӳ���һ����ֵ.
	
	```
	[[_textField.rac_textSignal flattenMap:^RACStream *(id value) {
        
        // block����ʱ�����ź�Դ������ʱ��
        
        // block���ã��ı��źŵ�����
        
        // ����RACReturnSignal
        return [RACReturnSignal return:[NSString stringWithFormat:@"�ź����ݣ�%@", value]];
        
    }] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    ```
- **�ײ�ʵ��**

     0. **flattenMap**�ڲ����� `bind` ����ʵ�ֵ�,**flattenMap**��block�ķ���ֵ������Ϊbind��bindBlock�ķ���ֵ��
     1. �����İ��źţ��ͻ����� `bindBlock`��
     2. ��Դ�źŷ������ݣ��ͻ����` bindBlock(value, *stop)`
     3. ����`bindBlock`���ڲ��ͻ���� **flattenMap** �� bloc k��**flattenMap** ��block���ã����ǰѴ���õ����ݰ�װ���źš�
     4. ���ص��ź����ջ���Ϊ `bindBlock` �еķ����źţ����� `bindBlock` �ķ����źš�
     5. ���� `bindBlock` �ķ����źţ��ͻ��õ����źŵĶ����ߣ��Ѵ�����ɵ��ź����ݷ��ͳ�����
	
###### Map

- **����**
 
	��Դ�źŵ�ֵӳ���һ���µ�ֵ

	
- **ʹ�ò���**
     1. ����һ��block,�����Ƿ��ض��󣬲����� `value`
     2. `value`����Դ�źŵ����ݣ�ֱ���õ�Դ�źŵ�����������
     3. �Ѵ���õ����ݣ�ֱ�ӷ��ؾͺ��ˣ����ð�װ���źţ����ص�ֵ������ӳ���ֵ��
    
- **ʹ��**

	�����ı�������ݸı䣬�ѽṹ����ӳ���һ����ֵ.
     
    ```
	[[_textField.rac_textSignal map:^id(id value) {
       
       // ƴ����󣬷��ض���
        return [NSString stringWithFormat:@"�ź�����: %@", value];
        
    }] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
	```
- **�ײ�ʵ��**:
     0. Map�ײ���ʵ�ǵ��� `flatternMa`p,`Map` ��block�еķ��ص�ֵ����Ϊ `flatternMap` ��block�е�ֵ
     1. �����İ��źţ��ͻ����� `bindBlock` 
     3. ��Դ�źŷ������ݣ��ͻ���� `bindBlock(value, *stop)`
     4. ���� `bindBlock` ���ڲ��ͻ���� `flattenMap��block`
     5. `flattenMap��block` �ڲ������ `Map` �е�block���� `Map` �е�block���ص����ݰ�װ�ɷ��ص��ź�
     5. ���ص��ź����ջ���Ϊ `bindBlock` �еķ����źţ����� `bindBlock` �ķ����ź�
     6. ���� `bindBlock` �ķ����źţ��ͻ��õ����źŵĶ����ߣ��Ѵ�����ɵ��ź����ݷ��ͳ�����

###### FlatternMap �� Map ������
-  **FlatternMap** �е�Block **�����ź�**�� 
2. **Map** �е�Block **���ض���**��
3. �����У�����źŷ�����ֵ **�����ź�** ��ӳ��һ��ʹ�� `Map`
4. ����źŷ�����ֵ **���ź�**��ӳ��һ��ʹ�� `FlatternMap`��



- `signalOfsignals`�� **FlatternMap**

	```
    // �����ź��е��ź�
    RACSubject *signalOfsignals = [RACSubject subject];
    RACSubject *signal = [RACSubject subject];

    [[signalOfsignals flattenMap:^RACStream *(id value) {

     // ��signalOfsignals��signals�����źŲŻ����

        return value;

    }] subscribeNext:^(id x) {

        // ֻ��signalOfsignals��signal�����źŲŻ���ã���Ϊ�ڲ�������bindBlock�з��ص��źţ�Ҳ����flattenMap���ص��źš�
        // Ҳ����flattenMap���ص��źŷ������ݣ��Ż���á�

        NSLog(@"signalOfsignals��%@",x);
    }];

    // �źŵ��źŷ����ź�
    [signalOfsignals sendNext:signal];

    // �źŷ�������
    [signal sendNext:@"hi"];
	
	```
	
#### ���

��Ͼ��ǽ�����źŰ���ĳ�ֹ������ƴ�ӣ��ϳ��µ��źš�

###### concat

- **����** 

	��**˳��ƴ��**�źţ�������źŷ�����ʱ����˳��Ľ����źš�
- **�ײ�ʵ��**
     1. ��ƴ���źű����ģ��ͻ����ƴ���źŵ�didSubscribe
     2. didSubscribe�У����ȶ��ĵ�һ��Դ�źţ�signalA��
     3. ��ִ�е�һ��Դ�źţ�signalA����didSubscribe
     4. ��һ��Դ�źţ�signalA��didSubscribe�з���ֵ���ͻ���õ�һ��Դ�źţ�signalA�������ߵ�nextBlock,ͨ��ƴ���źŵĶ����߰�ֵ���ͳ���.
     5. ��һ��Դ�źţ�signalA��didSubscribe�з�����ɣ��ͻ���õ�һ��Դ�źţ�signalA�������ߵ�completedBlock,���ĵڶ���Դ�źţ�signalB����ʱ��ż��signalB����
     6. ���ĵڶ���Դ�źţ�signalB��,ִ�еڶ���Դ�źţ�signalB����didSubscribe
     7. �ڶ���Դ�źţ�signalA��didSubscribe�з���ֵ,�ͻ�ͨ��ƴ���źŵĶ����߰�ֵ���ͳ���.
- **ʹ�ò���**

	1. ʹ��`concat:`ƴ���ź�
	2. ����ƴ���źţ��ڲ����Զ���ƴ��˳�����ź�
- **ʹ��**

	ƴ���ź� `signalA`�� `signalB`�� `signalC`
	
	```
	RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"Hello"];
        
        [subscriber sendCompleted];
        
        return nil;
    }];
    
    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"World"];
        
        [subscriber sendCompleted];
        
        return nil;
    }];
    
    RACSignal *signalC = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"!"];
        
        [subscriber sendCompleted];
        
        return nil;
    }];
    
    // ƴ�� A B, ��signalAƴ�ӵ�signalB��signalA������ɣ�signalB�Żᱻ���
    RACSignal *concatSignalAB = [signalA concat:signalB];
    
    // A B + C
    RACSignal *concatSignalABC = [concatSignalAB concat:signalC];
    
    
    // ����ƴ�ӵ��ź�, �ڲ��ᰴ˳���� A->B->C
    // ע�⣺��һ���źű��뷢����ɣ��ڶ����źŲŻᱻ����...
    [concatSignalABC subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
	```

######  then
- **����** 

	�������������źţ�����һ���ź���ɣ��Ż�����then���ص��źš�
- **�ײ�ʵ��**
	
	1. �ȹ��˵�֮ǰ���źŷ�����ֵ
	2. ʹ��concat����then���ص��ź�
	
- **ʹ��**

	```
   [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
      
      [subscriber sendNext:@1];
      
      [subscriber sendCompleted];
      
      return nil;
      
    }] then:^RACSignal *{
      
      	return [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
          
          [subscriber sendNext:@2];
          
          return nil;
      }];
      
    }] subscribeNext:^(id x) {
      
      // ֻ�ܽ��յ��ڶ����źŵ�ֵ��Ҳ����then�����źŵ�ֵ
      NSLog(@"%@", x);
      
    }];
    
    ///
    �����2
	```
- **ע��**

	ע��ʹ��`then`��֮ǰ�źŵ�ֵ�ᱻ���Ե�.

###### merge
- **����** 
	
	�ϲ��ź�,�κ�һ���źŷ������ݣ����ܼ�����.
- **�ײ�ʵ��**

     1. �ϲ��źű����ĵ�ʱ�򣬾ͻ���������źţ����ҷ�����Щ�źš�
     2. ÿ����һ���źţ�����źžͻᱻ����
     3. Ҳ���Ǻϲ��ź�һ�����ģ��ͻᶩ���������е��źš�
     4. ֻҪ��һ���źű������ͻᱻ������
- **ʹ��**

	```
	RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"A"];
        
        return nil;
    }];

    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"B"];
        
        return nil;
    }];

    // �ϲ��ź�, �κ�һ���źŷ������ݣ����ܼ�����
    RACSignal *mergeSianl = [signalA merge:signalB];

    [mergeSianl subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    // ���
	2017-01-03 13:29:08.013 ReactiveCocoa����[3627:718315] A
	2017-01-03 13:29:08.014 ReactiveCocoa����[3627:718315] B

    
	```

###### zip

- **����** 
	
	�������ź�ѹ����һ���źţ�ֻ�е������ź� **ͬʱ** �����ź�����ʱ�����Ұ������źŵ����ݺϲ���һ��Ԫ�飬�Żᴥ��ѹ������next�¼���
- **�ײ�ʵ��**
	
	1. ����ѹ���źţ��ڲ��ͻ��Զ�����signalA��signalB
	2. ÿ��signalA����signalB�����źţ��ͻ��ж�signalA��signalB��û�з������źţ��оͻ��ÿ���ź� ��һ�� ������ֵ��װ��Ԫ�鷢��
	     
- **ʹ��**

	```
	RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"A1"];
        [subscriber sendNext:@"A2"];
        
        return nil;
    }];
    
    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"B1"];
        [subscriber sendNext:@"B2"];
        [subscriber sendNext:@"B3"];
        
        return nil;
    }];
    
    RACSignal *zipSignal = [signalA zipWith:signalB];
    
    [zipSignal subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
	
	// ���
	2017-01-03 13:48:09.234 ReactiveCocoa����[3997:789720] zipWith: <RACTuple: 0x600000004df0> (
    A1,
    B1
	)
	2017-01-03 13:48:09.234 ReactiveCocoa����[3997:789720] zipWith: <RACTuple: 0x608000003410> (
    A2,
    B2
	)
	```
	
	
###### combineLatest
- **����** 
	
	������źźϲ������������õ������ź����һ��ֵ,����ÿ���ϲ���signal���ٶ��й�һ��sendNext���Żᴥ���ϲ����źš�

- **�ײ�ʵ��**
	
 	1. ������źű����ģ��ڲ����Զ�����signalA��signalB,���������źŶ��������ݣ��Żᱻ������
 	2. ���Ұ������źŵ� ���һ�� ���͵�ֵ��ϳ�Ԫ�鷢����
	     
- **ʹ��**

	```
	RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"A1"];
        [subscriber sendNext:@"A2"];
        
        return nil;
    }];
    
    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"B1"];
        [subscriber sendNext:@"B2"];
        [subscriber sendNext:@"B3"];
        
        return nil;
    }];
    
    RACSignal *combineSianal = [signalA combineLatestWith:signalB];
    
    [combineSianal subscribeNext:^(id x) {
        
        NSLog(@"combineLatest:%@", x);
    }];
	
	// ���
	2017-01-03 13:48:09.235 ReactiveCocoa����[3997:789720] combineLatest:<RACTuple: 0x60800000e150> (
    A2,
    B1
	)
	2017-01-03 13:48:09.235 ReactiveCocoa����[3997:789720] combineLatest:<RACTuple: 0x600000004db0> (
    A2,
    B2
	)
	2017-01-03 13:48:09.236 ReactiveCocoa����[3997:789720] combineLatest:<RACTuple: 0x60800000e180> (
    A2,
    B3
	)
	```
	
- **ע��**

	**combineLatest**��**zip**�÷����ƣ�����ÿ���ϲ���signal���ٶ��й�һ��sendNext���Żᴥ���ϲ����źš�
	
	������ͼ��
	
	![](https://ww2.sinaimg.cn/large/006y8lVagw1fbdf6cyez6j30id0kkabf.jpg)


###### reduce   

- **����** 
	
	���źŷ���Ԫ���ֵ�ۺϳ�һ��ֵ
- **�ײ�ʵ��**
	
 	1. ���ľۺ��źţ�
 	2. ÿ�������ݷ������ͻ�ִ��reduceblcok�����ź�����ת����reduceblcok���ص�ֵ��
	     
- **ʹ��**

     �������÷�����������ھۺϣ�`combineLatest:(id<NSFastEnumeration>)signals reduce:(id (^)())reduceBlock`
     
     reduce�е�block���:
     
     reduceblcok�еĲ������ж����ź���ϣ�reduceblcok���ж��ٲ�����ÿ����������֮ǰ�źŷ���������
     reduceblcok�ķ���ֵ���ۺ��ź�֮������ݡ�



	```
	    RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"A1"];
        [subscriber sendNext:@"A2"];
        
        return nil;
    }];
    
    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"B1"];
        [subscriber sendNext:@"B2"];
        [subscriber sendNext:@"B3"];
        
        return nil;
    }];
    
    
    RACSignal *reduceSignal = [RACSignal combineLatest:@[signalA, signalB] reduce:^id(NSString *str1, NSString *str2){
        
        return [NSString stringWithFormat:@"%@ %@", str1, str2];
    }];
    
    [reduceSignal subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    // ���
    2017-01-03 15:42:41.803 ReactiveCocoa����[4248:1264674] A2 B1
	2017-01-03 15:42:41.803 ReactiveCocoa����[4248:1264674] A2 B2
	2017-01-03 15:42:41.803 ReactiveCocoa����[4248:1264674] A2 B3
    
	```
	
#### ����

���˾��ǹ����ź��е� �ض�ֵ �����߹���ָ�� ���ʹ��� ���źš�

###### filter

- **����**

	�����źţ�ʹ�������Ի�ȡ�����������ź�.
	
	block�ķ���ֵ��Boolֵ������`NO`����˸��ź�
	
- **ʹ��**

	```
	// ����:
	// ÿ���źŷ���������ִ�й��������ж�.
	[[_textField.rac_textSignal filter:^BOOL(NSString *value) {
        
        NSLog(@"ԭ�ź�: %@", value);

        // ���� ���� <= 3 ���ź�
        return value.length > 3;
        
    }] subscribeNext:^(id x) {
        
        NSLog(@"���ȴ���3���źţ�%@", x);
    }];
    
    // ��_textField�����12345
	// ���
	2017-01-03 16:36:54.938 ReactiveCocoa����[4714:1552910] ԭ�ź�: 1
	2017-01-03 16:36:55.383 ReactiveCocoa����[4714:1552910] ԭ�ź�: 12
	2017-01-03 16:36:55.706 ReactiveCocoa����[4714:1552910] ԭ�ź�: 123
	2017-01-03 16:36:56.842 ReactiveCocoa����[4714:1552910] ԭ�ź�: 1234
	2017-01-03 16:36:56.842 ReactiveCocoa����[4714:1552910] ���ȴ���3���źţ�1234
	2017-01-03 16:36:58.350 ReactiveCocoa����[4714:1552910] ԭ�ź�: 12345
	2017-01-03 16:36:58.351 ReactiveCocoa����[4714:1552910] ���ȴ���3���źţ�12345
	```
	
###### ignore

- **����**

	����ĳЩ�ź�.
	
- **ʹ��**

- **����**

	����ĳЩֵ���ź�.
	
	�ײ������ `filter` �� ����ֵ���бȽϣ�����ȷ����� `NO`
	
- **ʹ��**

	```
  	// �ڲ�����filter���ˣ����Ե��ַ�Ϊ @��1����ֵ
[[_textField.rac_textSignal ignore:@"1"] subscribeNext:^(id x) {

 	 NSLog(@"%@",x);
}];


	```

###### distinctUntilChanged

- **����**

	����һ�ε�ֵ�͵�ǰ��ֵ�����Եı仯�ͻᷢ���źţ�����ᱻ���Ե���
	
- **ʹ��**

	```
	[[_textField.rac_textSignal distinctUntilChanged] subscribeNext:^(id x) {
        
        NSLog(@"%@",x);
    }];
	```
	
###### skip	

- **����**

	���� **��N��** �ķ��͵��ź�.
	
- **ʹ��**
	
	```
// ��ʾ�����һ�Σ����ᱻ��������������һ�η������ź�
[[_textField.rac_textSignal skip:1] subscribeNext:^(id x) {

   NSLog(@"%@",x);
}];
	```



##### take
- **����**

	ȡ **ǰN��** �ķ��͵��ź�.
- **ʹ��**

	```
	RACSubject *subject = [RACSubject subject] ;
    
    // ȡ ǰ���� ���͵��ź�
    [[subject take:2] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    [subject sendNext:@1];
    [subject sendNext:@2];
    [subject sendNext:@3];
    
    // ���
	2017-01-03 17:35:54.566 ReactiveCocoa����[4969:1677908] 1
	2017-01-03 17:35:54.567 ReactiveCocoa����[4969:1677908] 2
	```

###### takeLast

- **����**

	ȡ **���N��** �ķ��͵��ź�
	
	ǰ�������������߱��������� `sendCompleted`����Ϊֻ����ɣ���֪���ܹ��ж����ź�.
	
- **ʹ��**	

	```
	RACSubject *subject = [RACSubject subject] ;
    
    // ȡ ������ ���͵��ź�
    [[subject takeLast:2] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    [subject sendNext:@1];
    [subject sendNext:@2];
    [subject sendNext:@3];
    
    // ���� �������
    [subject sendCompleted];
	```

###### takeUntil

- **����**

	��ȡ�ź�ֱ��ĳ���ź�ִ�����
- **ʹ��**	

	```
	// �����ı���ĸı�ֱ����ǰ��������
[_textField.rac_textSignal takeUntil:self.rac_willDeallocSignal];
	```
	
###### switchToLatest
- **����**

	����signalOfSignals���źŵ��źţ�����ʱ���ź�Ҳ�ᷢ���źţ�����signalOfSignals�У���ȡsignalOfSignals���͵������źš�
	
- **ע��**

	switchToLatest��ֻ�������ź��е��ź�

- **ʹ��**	

	```
	RACSubject *signalOfSignals = [RACSubject subject];
    RACSubject *signal = [RACSubject subject];
    
    // ��ȡ�ź����ź���������źţ���������������źš�
    [signalOfSignals.switchToLatest subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    [signalOfSignals sendNext:signal];
    [signal sendNext:@1];
	```

#### ����

������� `doNext` �� `doCompleted` ��������������Ҫ���� ִ��`sendNext` ���� `sendCompleted`֮ǰ����ִ����Щ������Block��

###### doNext 
	
ִ��`sendNext`֮ǰ������ִ�����`doNext`�� Block

###### doCompleted

ִ��`sendCompleted`֮ǰ������ִ����`doCompleted`��`Block`

```
[[[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
    
    [subscriber sendNext:@"hi"];
    
    [subscriber sendCompleted];
    
    return nil;
    
}] doNext:^(id x) {
    
    // ִ�� [subscriber sendNext:@"hi"] ֮ǰ�������� Block
    NSLog(@"doNext");
    
}] doCompleted:^{
    
    // ִ�� [subscriber sendCompleted] ֮ǰ������� Block
    NSLog(@"doCompleted");
}] subscribeNext:^(id x) {
    
    NSLog(@"%@", x);
}];
    

```

#### �߳�

**ReactiveCocoa** �е��̲߳��� ���� `deliverOn` �� `subscribeOn`�����֣��� *���ݵ�����* �� �����ź�ʱ *block�еĴ���* �л���ָ�����߳���ִ�С�

###### deliverOn

- **����**

	���ݴ����л����ƶ��߳��У���������ԭ���߳���,���ڴ����ź�ʱblock�еĴ����֮Ϊ�����á�
- **ʹ��**

	```
	// �����߳���ִ��
	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        
        [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
            NSLog(@"%@", [NSThread currentThread]);
            
            [subscriber sendNext:@123];
            
            [subscriber sendCompleted];
            
            return nil;
        }]
          deliverOn:[RACScheduler mainThreadScheduler]]
          
         subscribeNext:^(id x) {
         
             NSLog(@"%@", x);
             
             NSLog(@"%@", [NSThread currentThread]);
         }];
    });
    
    // ���
2017-01-04 10:35:55.415 ReactiveCocoa����[1183:224535] <NSThread: 0x608000270f00>{number = 3, name = (null)}
2017-01-04 10:35:55.415 ReactiveCocoa����[1183:224482] 123
2017-01-04 10:35:55.415 ReactiveCocoa����[1183:224482] <NSThread: 0x600000079bc0>{number = 1, name = main}
	```
	
	���Կ���`������`�� *���߳�* ��ִ�У��� `���ݵ�����` �� *���߳�* �н���


###### subscribeOn
- **����**

	**subscribeOn**���ǽ� `���ݴ���` �� `������` �����л���ָ���߳���
- **ʹ��**

	```
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        
        [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
            NSLog(@"%@", [NSThread currentThread]);
            
            [subscriber sendNext:@123];
            
            [subscriber sendCompleted];
            
            return nil;
        }]
          subscribeOn:[RACScheduler mainThreadScheduler]] //���ݵ����ݵ����߳���
         subscribeNext:^(id x) {
         
             NSLog(@"%@", x);
             
             NSLog(@"%@", [NSThread currentThread]);
         }];
    });	
	//
2017-01-04 10:44:47.558 ReactiveCocoa����[1243:275126] <NSThread: 0x608000077640>{number = 1, name = main}
2017-01-04 10:44:47.558 ReactiveCocoa����[1243:275126] 123
2017-01-04 10:44:47.558 ReactiveCocoa����[1243:275126] <NSThread: 0x608000077640>{number = 1, name = main}
	```
	
	`���ݴ���` �� `������` ���л����� *���߳�* ִ��
	
#### ʱ��

ʱ������ͻ������źų�ʱ����ʱ����ʱ��

###### interval ��ʱ
- **����**

	��ʱ��ÿ��һ��ʱ�䷢���ź�
	
	```
	// ÿ��1�뷢���źţ�ָ����ǰ�߳�ִ��
	[[RACSignal interval:1 onScheduler:[RACScheduler currentScheduler]] subscribeNext:^(id x) {
        
        NSLog(@"��ʱ:%@", x);
    }];
    
	// ���
	2017-01-04 13:48:55.196 ReactiveCocoa����[1980:492724] ��ʱ:2017-01-04 05:48:55 +0000
	2017-01-04 13:48:56.195 ReactiveCocoa����[1980:492724] ��ʱ:2017-01-04 05:48:56 +0000
	2017-01-04 13:48:57.196 ReactiveCocoa����[1980:492724] ��ʱ:2017-01-04 05:48:57 +0000
	```


###### timeout ��ʱ

- **����**

	��ʱ��������һ���ź���һ����ʱ����Զ�����
	
	```
	RACSignal *signal = [[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        // �������źţ�ģ�ⳬʱ״̬
        // [subscriber sendNext:@"hello"];
        //[subscriber sendCompleted];
        
        return nil;
    }] timeout:1 onScheduler:[RACScheduler currentScheduler]];// ����1�볬ʱ
    
    [signal subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    } error:^(NSError *error) {
        
        NSLog(@"%@", error);
    }];
    
    // ִ�д��� 1��� �����
    2017-01-04 13:48:55.195 ReactiveCocoa����[1980:492724] Error Domain=RACSignalErrorDomain Code=1 "(null)"
	```

###### delay ��ʱ
- **����**

	��ʱ���ӳ�һ��ʱ������ź�
	
	```
	RACSignal *signal2 = [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"�ӳ����"];
        
        return nil;
    }] delay:2] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    // ִ�д��� 2��� ���
    2017-01-04 13:55:23.751 ReactiveCocoa����[2030:525038] �ӳ����
	```


#### �ظ�

###### retry

- **����**

	���ԣ�ֻҪ ���ʹ��� `sendError:`,�ͻ� ����ִ�� �����źŵ�Block ֱ���ɹ�
	
	```
	__block int i = 0;
    
    [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        if (i == 5) {
            
            [subscriber sendNext:@"Hello"];
            
        } else {
            
            // ���ʹ���
            NSLog(@"�յ�����:%d", i);
            [subscriber sendError:nil];
        }
        
        i++;
        
        return nil;
        
    }] retry] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
        
    } error:^(NSError *error) {
        
        NSLog(@"%@", error);
        
    }];

	// ���
2017-01-04 14:36:51.594 ReactiveCocoa����[2443:667226] �յ�������Ϣ:0
2017-01-04 14:36:51.595 ReactiveCocoa����[2443:667226] �յ�������Ϣ:1
2017-01-04 14:36:51.595 ReactiveCocoa����[2443:667226] �յ�������Ϣ:2
2017-01-04 14:36:51.596 ReactiveCocoa����[2443:667226] �յ�������Ϣ:3
2017-01-04 14:36:51.596 ReactiveCocoa����[2443:667226] �յ�������Ϣ:4
2017-01-04 14:36:51.596 ReactiveCocoa����[2443:667226] Hello

	```

###### replay

- **����**

	�طţ���һ���źű���ζ���,������������
	
	```
	RACSignal *signal = [[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@1];
        [subscriber sendNext:@2];
        
        return nil;
    }] replay];
    
    [signal subscribeNext:^(id x) {
        NSLog(@"%@", x);
    }];
    
    [signal subscribeNext:^(id x) {
        NSLog(@"%@", x);
    }];
    
    // ���
2017-01-04 14:51:01.934 ReactiveCocoa����[2544:706740] 1
2017-01-04 14:51:01.934 ReactiveCocoa����[2544:706740] 2
2017-01-04 14:51:01.934 ReactiveCocoa����[2544:706740] 1
2017-01-04 14:51:01.935 ReactiveCocoa����[2544:706740] 2
	```


###### throttle

- **����**

	����:��ĳ���źŷ��ͱȽ�Ƶ��ʱ������ʹ�ý�������ĳһ��ʱ�䲻�����ź����ݣ�����һ��ʱ���ȡ�źŵ��������ݷ�����
	
	```
	RACSubject *subject = [RACSubject subject];
    
    // ����1�룬1���������һ�����͵��ź�
    [[subject throttle:1] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    [subject sendNext:@1];
    [subject sendNext:@2];
    [subject sendNext:@3];
    
    // ���
    2017-01-04 15:02:37.543 ReactiveCocoa����[2731:758193] 3
	```

# MVVM�ܹ�˼��
---
����ΪʲôҪ�мܹ������ڳ��򿪷���ά��.

#### �����ļܹ�
- **MVC**

	M:ģ�� V:��ͼ C:������

- **MVVM**

	M:ģ�� V:��ͼ+������ VM:��ͼģ��

- **MVCS**

	 M:ģ�� V:��ͼ C:������ C:������

- [**VIPER**](http://www.cocoachina.com/ios/20140703/9016.html)

	V:��ͼ I:������ P:չʾ�� E:ʵ�� R:·��

#### MVVM����

- ģ��(M):������ͼ���ݡ�

- ��ͼ+������(V):չʾ���� + ���չʾ

- ��ͼģ��(VM):����չʾ��ҵ���߼���������ť�ĵ�������ݵ�����ͽ����ȵȡ�

# ʵսһ����¼����

#### ����
1. ���������ı��������
2. �����ݵ�¼����������ť���
3. ���ص�¼���

#### ����
1. ���������ҵ���߼�������������������
2. ��MVVM�ܹ��аѿ�������ҵ��ȫ����ȥVMģ�ͣ�Ҳ����ÿ����������Ӧһ��VMģ��.

#### ����
1. ����LoginViewModel�࣬�����¼����ҵ���߼�.
2. ���������Ӧ�ñ������˺ŵ���Ϣ������һ���˺�Accountģ��
3. LoginViewModelӦ�ñ������˺���ϢAccountģ�͡�
4. ��Ҫʱ�̼���Accountģ���е��˺ź�����ĸı䣬��ô������
5. �ڷ�RAC�����У�����ϰ�߸�ֵ����RAC�����У���Ҫ�ı俪��˼ά���ɸ�ֵת��Ϊ�󶨣�������һ��ʼ��ʼ����ʱ�򣬾͸�Accountģ���е����԰󶨣�������Ҫ��дset������
6. ÿ��Accountģ�͵�ֵ�ı䣬����Ҫ�жϰ�ť�ܷ�������VMģ����������������ṩһ���ܷ�����ť���ź�.
7. �����¼�ź���Ҫ�ж�Account���˺ź������Ƿ���ֵ����KVO����������ֵ�ĸı䣬�����Ǿۺϳɵ�¼�ź�.
8. ������ť�ĵ������VM����Ӧ�ø�VM����һ��RACCommand��ר�Ŵ����¼ҵ���߼�.
9. ִ����������ݰ�װ���źŴ��ݳ�ȥ
10. �����������źŵ����ݴ���
11. ���������ִ��ʱ��



#### ����Ч��

![��¼����](https://ww3.sinaimg.cn/large/006y8lVagw1fbgvoh8yu6j30bj0l43yz.jpg)

#### ����

`MyViewController.m`

```
#import "MyViewController.h"
#import "LoginViewModel.h"

@interface MyViewController ()

@property (nonatomic, strong) LoginViewModel *loginViewModel;

@property (weak, nonatomic) IBOutlet UITextField *accountField;

@property (weak, nonatomic) IBOutlet UITextField *pwdField;

@property (weak, nonatomic) IBOutlet UIButton *loginBtn;

@end

@implementation MyViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self bindModel];
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}



// ��ͼģ�Ͱ�
- (void)bindModel {

    // ��ģ�͵����԰��ź�
    //
    RAC(self.loginViewModel.account, account) = _accountField.rac_textSignal;
    RAC(self.loginViewModel.account, pwd) = _pwdField.rac_textSignal;
    
    RAC(self.loginBtn, enabled) = self.loginViewModel.enableLoginSignal;
    
    // ������¼���
    [[_loginBtn rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(id x) {
        
        [self.loginViewModel.LoginCommand execute:nil];
    }];
    
}
- (IBAction)btnTap:(id)sender {
    
    
}

#pragma mark - lazyLoad

- (LoginViewModel *)loginViewModel {
    
    if (nil == _loginViewModel) {
        _loginViewModel = [[LoginViewModel alloc] init];
    }
    
    return _loginViewModel;
}
```	
		
`LoginViewModel.h`

```
#import <UIKit/UIKit.h>

@interface Account : NSObject

@property (nonatomic, strong) NSString *account;
@property (nonatomic, strong) NSString *pwd;

@end


@interface LoginViewModel : UIViewController

@property (nonatomic, strong) Account *account;

// �Ƿ������¼���ź�
@property (nonatomic, strong, readonly) RACSignal *enableLoginSignal;

@property (nonatomic, strong, readonly) RACCommand *LoginCommand;

@end

```

`LoginViewModel.m`

```
#import "LoginViewModel.h"

@implementation Account

@end


@interface LoginViewModel ()

@end

@implementation LoginViewModel

- (instancetype)init {
    
    if (self = [super init]) {
        [self initialBind];
    }
    return self;
}

- (void)initialBind {

    // �����˺����Ըı䣬 �����Ǻϳ�һ���ź�
    _enableLoginSignal = [RACSubject combineLatest:@[RACObserve(self.account, account), RACObserve(self.account, pwd)] reduce:^id(NSString *accout, NSString *pwd){
        
        return @(accout.length && pwd.length);
    }];
    
    // ����ҵ���߼�
    _LoginCommand = [[RACCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
        
        NSLog(@"����˵�¼");
        return [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
            
            // ģ�������ӳ�

            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(0.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                
                // ���ص�¼�ɹ� ���ͳɹ��ź�
                [subscriber sendNext:@"��¼�ɹ�"];
            });
            
            return nil;
        }];
    }];
    
    
    // ������¼����������
    [_LoginCommand.executionSignals.switchToLatest subscribeNext:^(id x) {
       
        if ([x isEqualToString:@"��¼�ɹ�"]) {
            NSLog(@"��¼�ɹ�");
        }
        
    }];
    
    [[_LoginCommand.executing skip:1] subscribeNext:^(id x) {
        
        if ([x isEqualToNumber:@(YES)]) {
            
            NSLog(@"���ڵ�½...");
        } else {
            
        // ��¼�ɹ�
        NSLog(@"��½�ɹ�");
        
        }
        
    }];
}

#pragma mark - lazyLoad

- (Account *)account
{
    if (_account == nil) {
        _account = [[Account alloc] init];
    }
    return _account;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
}

@end

```

# ʵս����������������

#### ����
1. ����һ���������ݣ������󵽵�������`tableView`��չʾ
2. ������Ϊ����ͼ����������ؽ����URL��url:https://api.douban.com/v2/book/search?q=��մ�

#### ����
1. ���������ҵ���߼�������**������**������
2. �������󽻸�**MV**ģ�ʹ���

#### ����

1. �������ṩһ����ͼģ�ͣ�requesViewModel������������ҵ���߼�
2. VM�ṩһ�������������ҵ���߼�
3. �ڴ��������block�У���������װ��һ���źţ�������ɹ���ʱ�򣬾ͻ�����ݴ��ݳ�ȥ��
4. �������ݳɹ���Ӧ�ð��ֵ�ת����ģ�ͣ����浽��ͼģ���У����������þ�ֱ�Ӵ���ͼģ���л�ȡ��

#### ����

����������ͼƬ�����õ���[AFNetworking](https://github.com/AFNetworking/AFNetworking) �� [SDWebImage](https://github.com/rs/SDWebImage),������Pods�е��롣

```
platform :ios, '8.0'

target 'ReactiveCocoa����' do

use_frameworks!
pod 'ReactiveCocoa', '~> 2.5'
pod 'AFNetworking'
pod 'SDWebImage'
end
```

#### ����Ч��

![](https://ww3.sinaimg.cn/large/006y8lVagw1fbgw1xnz74j30bj0l4408.jpg)


#### ����

`SearchViewController.m`

```
#import "SearchViewController.h"
#import "RequestViewModel.h"

@interface SearchViewController ()<UITableViewDataSource>

@property (nonatomic, strong) UITableView *tableView;

@property (nonatomic, strong) RequestViewModel *requesViewModel;

@end

@implementation SearchViewController

- (RequestViewModel *)requesViewModel
{
    if (_requesViewModel == nil) {
        _requesViewModel = [[RequestViewModel alloc] init];
    }
    return _requesViewModel;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
    
    self.tableView = [[UITableView alloc] initWithFrame:self.view.frame];
    
    self.tableView.dataSource = self;
    
    [self.view addSubview:self.tableView];
    
    //
    RACSignal *requesSiganl = [self.requesViewModel.reuqesCommand execute:nil];
    
    [requesSiganl subscribeNext:^(NSArray *x) {
        
        self.requesViewModel.models = x;
        
        [self.tableView reloadData];
    }];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.requesViewModel.models.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *ID = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    if (cell == nil) {
        
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }
    
    Book *book = self.requesViewModel.models[indexPath.row];
    cell.detailTextLabel.text = book.subtitle;
    cell.textLabel.text = book.title;
    
    [cell.imageView sd_setImageWithURL:[NSURL URLWithString:book.image] placeholderImage:[UIImage imageNamed:@"cellImage"]];
    
    
    return cell;
}
@end
```

`RequestViewModel.h`

```
#import <Foundation/Foundation.h>

@interface Book : NSObject

@property (nonatomic, copy) NSString *subtitle;
@property (nonatomic, copy) NSString *title;
@property (nonatomic, copy) NSString *image;

@end

@interface RequestViewModel : NSObject

// ��������
@property (nonatomic, strong, readonly) RACCommand *reuqesCommand;

//ģ������
@property (nonatomic, strong) NSArray *models;


@end
```

`RequestViewModel.m`

```
#import "RequestViewModel.h"

@implementation Book

- (instancetype)initWithValue:(NSDictionary *)value {
    
    if (self = [super init]) {
        
        self.title = value[@"title"];
        self.subtitle = value[@"subtitle"];
        self.image = value[@"image"];
    }
    return self;
}

+ (Book *)bookWithDict:(NSDictionary *)value {
    
    return [[self alloc] initWithValue:value];
}



@end

@implementation RequestViewModel

- (instancetype)init
{
    if (self = [super init]) {
        
        [self initialBind];
    }
    return self;
}


- (void)initialBind
{
    _reuqesCommand = [[RACCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
        
      RACSignal *requestSiganl = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
          
          NSMutableDictionary *parameters = [NSMutableDictionary dictionary];
          parameters[@"q"] = @"��մ�";
          
          //
          [[AFHTTPSessionManager manager] GET:@"https://api.douban.com/v2/book/search" parameters:parameters progress:^(NSProgress * _Nonnull downloadProgress) {
              
              NSLog(@"downloadProgress: %@", downloadProgress);
          } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
              
              // ��������ɹ��ͽ����ݷ��ͳ�ȥ
              NSLog(@"responseObject:%@", responseObject);
              
              [subscriber sendNext:responseObject];
              
              [subscriber sendCompleted];
              
          } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
              
              NSLog(@"error: %@", error);
          }];
          
          
         return nil;
      }];
        
        // �ڷ��������ź�ʱ���������е��ֵ�ӳ���ģ���źţ����ݳ�ȥ
        return [requestSiganl map:^id(NSDictionary *value) {
            
            NSMutableArray *dictArr = value[@"books"];
            
            NSArray *modelArr = [[dictArr.rac_sequence map:^id(id value) {
                
                return [Book bookWithDict:value];
                
            }] array];
            
            return modelArr;
            
        }];
        
    }];
}


@end

```

>�����GitHub��<https://github.com/qiubaiying/ReactiveCocoa_Demo>