## Bubble Sort
   最近在学习算法，记录在学习过程中一些经验的积累，本次主要学习的是冒泡算法，也叫Bubble Sort, 请一定记录英文名，不要再别人问你的时候，一脸茫（meng）然（bi）。
   
   "实践是检查真理唯一标注" 这句伟大的名言让我深有体会，在学完后，原本以为很简单，就不太在意，今天突然想动手写下这个算法，居然出了好多问题，真实惭愧啊！
   
   首先冒泡算法原理是每次仅比较相邻两个数，大小顺序不对的话即交换；然后，再写的过程，突然领悟到这个代码还可以优化，Very Happy!
   
   分析下算法的时间复杂度：循环遍历n次，每次比较相邻两个数，每次都能排序一个数，所以是 n + n-1 + ... + 1 = (n + 1)*n/2 => O(n^2)
   
   空间复杂度：不需要额外空间做为辅助，所以是O(1)

#### 优化前：
```
	private void bubbleSort(int[] num) {
		int length = num.length;
		int i = 0;
		for(;i<length;i++) {
			for(int k=0;k<length-1;k++) {
				if(num[k] > num[k+1]) {
					int temp = num[k];
					num[k] = num[k+1];
					num[k+1] = temp;
				}
			}
		}
		
		System.out.println("Loop count：" + i);
	}
```

#### 优化后：
```
	private void bubbleSort(int[] num) {
		int length = num.length;
		boolean hasSwap = true;
		int i = 0;
		for(;i<length;i++) {
			if(!hasSwap) {
				break;
			}
			hasSwap = false;
		
			for(int k=0;k<length-1;k++) {
				if(num[k] > num[k+1]) {
					int temp = num[k];
					num[k] = num[k+1];
					num[k+1] = temp;
					hasSwap = true;
				}
			}
		}
		
		System.out.println("Loop count：" + i);
	}
```
