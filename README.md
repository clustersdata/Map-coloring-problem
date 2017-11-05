# Map-coloring-problem

Map coloring problem

【参考程序１】

const lin:array[1..12,1..12] of 0..1  {区域相邻数组,1表示相邻}

      =((0,1,1,1,1,1,0,0,0,0,0,0),
      
	(1,0,1,0,0,1,1,1,0,0,0,0),
  
	(1,1,0,1,0,0,0,1,1,0,0,0),
  
	(1,0,1,0,1,0,1,0,1,1,0,0),
  
	(1,0,0,1,0,1,0,0,0,1,1,0),
  
	(1,1,0,0,1,0,1,0,0,0,1,0),
  
	(0,1,0,0,0,1,0,1,0,0,1,1),
  
	(0,1,1,0,0,0,1,0,1,0,0,1),
  
	(0,0,1,1,0,0,0,1,0,1,0,1),
  
	(0,0,0,1,1,0,0,0,1,0,1,1),
  
	(0,0,0,0,1,1,1,0,0,1,0,1),
  
	(0,0,0,0,0,0,1,1,1,1,1,1));
  
  
var  color:array[1..12] of byte;   {color数组放已填的颜色}

     total:integer;
     
function ok(dep,i:byte):boolean;  {判断选用色i是否可用}

var k:byte;			  {条件:相邻的区域颜色不能相同}

begin

  for k:=1 to dep do
  
      if (lin[dep,k]=1) and (i=color[k]) then begin ok:=false;exit;end;
      
  ok:=true;
  
end;


procedure output;     {输出}

var k:byte;

begin

  for k:=1 to 12 do write(color[k],' ');writeln;
  
  total:=total+1;
  
end;

procedure find(dep:byte); {参数dep:当前正在填的层数}

var i:byte;

begin

  for i:=1 to 4 do begin       {每个区域都可能是1-4种颜色}
  
   if ok(dep,i) then  begin
   
	    color[dep]:=i;
      
	    if dep=12 then output else find(dep+1);
      
	    color[dep]:=0;     {恢复初始状态,以便下一次搜索}
      
	    end;
      
  end;
  
end;

begin

 total:=0; {总数初始化}
 
 fillchar(color,sizeof(color),0);
 
 find(1);
 
 writeln('total:=',total);
 
end.

【参考程序２】

const	  {lin数组:代表区域相邻情况}

 lin:array[1..12] of set of  1..12 =
 
	 ([2,3,4,5,6],[1,3,6,7,8],[1,2,4,8,9],[1,3,5,9,10],[1,4,6,10,11],
   
	  [1,2,5,7,11],[12,8,11,6,2],[12,9,7,2,3],[12,8,10,3,4],
    
	  [12,9,11,4,5],[12,7,10,5,6],[7,8,9,10,11]);
    
color:array[1..4] of char=('r','y','b','g');

var a:array[1..12] of byte; {因有12个区域,故a数组下标为1-12}

    total:integer;
    
function ok(dep,i:integer):boolean; {判断第dep块区域是否可填第i种色}

var j:integer;	{ j 为什么设成局部变量?}

begin

     ok:=true;
     
     for j:=1 to 12 do
     
	 if (j in lin[dep]) and (a[j]=i) then ok:=false;
   
end;


procedure output; {输出过程}

 var j:integer;  { j 为什么设成局部变量?}
 
 
 begin
 
	  inc(total);  {方案总数加1}
    
	  write(total:4); {输出一种方案}
    
	  for j:=1 to 12 do write(color[a[j]]:2);writeln;
    
end;

procedure find(dep:byte);

var i:byte; { i 为什么设成局部变量?}

begin

      for i:=1 to 4 do	     {每一区域均可从4种颜色中选一}
      
	  begin
    
	   if ok(dep,i) then begin    {可填该色}
     
	      a[dep]:=i;   {第dep块区域填第i种颜色}
        
	      if (dep=12) then output	 {填完12个区域}
        
			  else find(dep+1); {未填完}
        
	       a[dep]:=0;  {取消第dep块区域已填的颜色}
         
	      end;
        
	  end;
    
end;

begin {主程序}

     fillchar(a,sizeof(a),0);  {记得要给变量赋初值!}
     
     total:=0;
     
     find(1);
     
     writeln('End.');
     
end.
