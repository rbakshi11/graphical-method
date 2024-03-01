# graphical-method
% Max z=x1+5x2 ;
% -x1+3x2<=10;
% x1+x2<=6;
% x1-x2<=2;
% x1,x2>=0;
% x1=0:max(b);
% x12=(10-(-x1))/3
% x12=(b(1)-A(1,1)*x1)/A(1,2)

A=[-1 3;1 1;1 -1]
B=[10;6;2]
C=[1 5]
x1=0:max(B)
x12=(B(1)-A(1,1)*x1)/A(1,2)
x22=(B(2)-A(2,1)*x1)/A(2,2)
x32=(B(3)-A(3,1)*x1)/A(3,2)
x12=max(0,x12)
x22=max(0,x22)
x32=max(0,x32)
plot(x1,x12,x1,x22,x1,x32)

cx1=find(x1==0)
c1=find(x12==0)
line1=[x1([cx1 c1]);x12([cx1 c1])]'
c2=find(x22==0)
line2=[x1([cx1 c2]);x22([cx1 c2])]'
c3=find(x32==0)
line3=[x1([cx1 c3]);x32([cx1 c3])]'
cor=[line1;line2;line3]

sol=[0 0]
for i=1:size(A,1)
    A1=A(i,:)
    B1=B(i,:)
    for j=i+1:size(A,1)
        A2=A(j,:)
        B2=B(j,:)
        
        A3=[A1;A2]
        B3=[B1;B2]
        x=(A3\B3)';
        sol=[sol;x]
    end
end

point=[sol;cor]
for i=1:size(point,1)
    const1(i)=A(1,1)*point(i,1)+A(1,2)*point(i,2)-B(1)
     const2(i)=A(2,1)*point(i,1)+A(2,2)*point(i,2)-B(2)
      const3(i)=A(3,1)*point(i,1)+A(3,2)*point(i,2)-B(3)
end

s1 = find(const1>0)
s2 = find(const2>0)
s3 = find(const3>0)
s=[s1 s2 s3]
point(s,:)=[]

z=point*C'
max(z)
[optimal index]=max(z)


fprintf('The optimal value is %d and index is %f,%f',optimal,point(index,:))
