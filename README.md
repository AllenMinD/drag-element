# drag-element
元素拖拽实现
每一次鼠标拖拽元素，背后其实都经历了3个鼠标事件：`鼠标按下（mousedown）`、`鼠标移动（mousemove）`、`鼠标松开（mouseup）`  

* 鼠标事件1—— **鼠标按下**  
	第一步，当鼠标在拖拽的元素上按下时，要完成两件事：  
	- 计算鼠标指针相对于拖拽元素左上角的坐标距离  
	- 标记元素为**可拖拽状态**  
	
	代码：  
	<pre>
	box.onmousedown = function(event) {
	    var e = event || window.event;
        var mouseCurrentX = e.clientX;  //鼠标当前位置
        var mouseCurrentY = e.clientY; 
	    mouseOffsetX = mouseCurrentX - box.offsetLeft; //鼠标相对于拖拽元素左边框的距离
	    mouseOffsetY = mouseCurrentY - box.offsetTop;  //鼠标相对于拖拽元素上边框的距离
	    isDragging = true;  //标记元素为可拖拽状态 
	}
	</pre>
* 鼠标事件2—— **鼠标移动**  
	第二步，当鼠标正在移动（拖拽元素）时，同样要完成两件事（但会复杂一点）：  
	- 更新拖拽元素的位置  
	- 判断拖拽元素是否有超出边界（客户区），不能让拖拽元素超出客户区  
	
	代码：  
	<pre>
	document.onmousemove = function(event) {
	    var e = event || window.event;
	    var mouseCurrentX = e.clientX;  //鼠标当前位置
        var mouseCurrentY = e.clientY;

        //拖拽元素的新位置
        var moveX  = 0;  // 元素左边框相对于可视区（客户区）左边的距离
        var moveY  = 0;  // 元素上边框相对于可视区（客户区）上边的距离

        if (isDragging === true) {
            //拖拽元素的新位置 = 鼠标当前坐标 - 鼠标相对于拖拽元素的坐标
            moveX = mouseCurrentX - mouseOffsetX;  
            moveY = mouseCurrentY - mouseOffsetY;

            //拖动范围：moveX > 0 并且 moveX < (可视区（客户区）最大宽度 - 拖拽元素的宽度)
            //         movey > 0 并且 movey < (可视区（客户区）最大高度 - 拖拽元素的高度)
            var pageWidth = document.documentElement.clientWidth;  //浏览器窗口的大小
            var pageHeight = document.documentElement.clientHeight;

            var maxX = pageWidth - box.offsetWidth;
            var maxY = pageHeight - box.offsetHeight;

            moveX = Math.min( maxX, Math.max(0, moveX) );  // 0 < moveX < maxX
            moveY = Math.min( maxY, Math.max(0, moveY) );  // 0 < moveY < maxY

            box.style.left = moveX + "px";
            box.style.top = moveY + "px";
        }		
	}
	</pre>
* 鼠标事件3—— **鼠标松开**  
	第三步，当鼠标松开时，只需要另拖拽元素的状态变为不可拖拽即可。  
	代码：  
	<pre>
    document.onmouseup = function() {
      isDragging = false;
    };
	</pre>
