# finance
#include <iostream>
#include <fstream>
#include <string.h>
#include <conio.h>
using namespace std;

class Money{
	public:
	char name[20];
	char date[10];
	int Inum; 
	int Onum;  
	int sum;
	Money *Next;
	void Input(){
		cout << "請輸入日期(格式：0101)：";
		cin >> date;
		cout << "請輸入收入/支出名稱："; 
		cin >> name;
		cout << "請輸入收入金額："; 
		cin >> Inum;
		cout << "請輸入支出金額："; 
		cin >> Onum;
		
		sum = Inum - Onum;
	}
	void ReadFile( istream & cin ) {
		cin >> date >> name >> Inum >> Onum;
	}
	void Show(){
		cout << "日期：" << date << endl << "名稱：" << name << endl <<"收入："<< Inum << endl << "支出：" << Onum << endl << "總額：" << sum << endl << endl << endl;
	}
};

//---------------------------------------------------------------------------------------//

class massage{
	public:
	massage();
	~massage();
	void ShowMenu();
	void Find();
	void Save();
	void ModifyItem();
	void RemoveItem();
	void Swap( Money * , Money * );
	void Sort();

	int ListCount();

	void Display(){
		for( Money *p = Head -> Next ; p != End ; p = p -> Next )
		p -> Show();
		cout << "輸入任意字符！繼續……";
		getch();
	}
	void AddItem(){
		End -> Input();
		End -> Next = new Money;
		End = End -> Next;
		cout << "已添加！" << endl;
		cout << "輸入任意字符！繼續……";
 		getch();
 	}
	private:
 	Money *Head , *End;
 	ifstream in;
 	ofstream out;
 	Money *FindItem( char *name ){
 		for( Money *p = Head; p -> Next != End ; p = p -> Next )
 			if( ! strcmp( p -> Next -> name , name ))
				return p;
 		return NULL;
 	}
 	Money *Finddate( char *date ){
 		for( Money *p = Head ; p -> Next != End ; p = p -> Next)
 			if( ! strcmp ( p -> Next -> date, date ))return p;
 		return NULL;
 	} 
};

//構造------------------------------------------------------------------//
massage::massage(){
	Head = new Money;
 	Head -> Next = new Money;
 	End = Head -> Next;
 	in.open( "sort.txt" );
 	if( ! in ){
 	cout << "這是一個新系統，無理財信息。請先輸入。" << endl;
 }
 	else{
		while( ! in.eof() ){
 			End -> ReadFile(in);
 			if(End -> name[0] == '\0' )
				break;
 			End -> Next = new Money;
 			End = End -> Next;
 		}	
 		in.close();
 		cout << "歡迎使用本系統！" << endl; 
 	}
} 

//析構-------------------------------------------------------//
massage::~massage(){
 	Save();
 	for( Money *temp; Head -> Next != End ;){
 		temp = Head -> Next;
 		Head -> Next = Head -> Next -> Next;
 		delete temp;
 	}
 	delete Head,End;
}

//菜單--------------------------------------------------------//
void massage::ShowMenu(){
 	cout<<"***********************************************"<<endl;
 	cout<<"*********       ☆ 理 財 系 統   ★   *********"<<endl;
 	cout<<"*********★ ★  ★★★★★★★★ ★ ★*********"<<endl;
 	cout<<"*********★ ☆   1.增加收入支出  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   2.顯示收入支出  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   3.排序統計金額  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   4.查找理財資料  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   5.刪除理財資料  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   6.修改理財資訊  ☆ ★*********"<<endl;
 	cout<<"*********★ ☆   0.安全退出系統  ☆ ★*********"<<endl;
 
 	cout<<"\n\t\t\n請選擇：";
}
 
//查找-------------------------------------------------------------// 
void massage::Find(){
 	char name[20] , date[10];
 	int x;
 	Money *p = NULL;
 	cout << "\n\t\t*********************************\n";
 	cout << "\t\t※ 1.照名稱查找\n\t\t※ 2.照日期查找";
 	cout << "\n\t\t*********************************\n請選擇：";
 	cin >> x;
 	switch( x ){
 		case 1:{
			cout << "請輸入要查找的理財名稱：";
			cin >> name;
	 		if( p = FindItem( name )){
	 			p -> Next -> Show();
	 			cout << "輸入任意字符！繼續……";
	 			getch();
	 		}
	 		else{
	 			cout << "沒有找到該名稱的資料！"<<'\n'<<endl;
	 			cout << "輸入任意字符！繼續……";
	 			getch();
	 		}
  		}
		break;
 		case 2:{
 		cout << "請輸入要查找的理財日期：";
		cin >> date;
 			if( p = Finddate( date )){
 				p -> Next -> Show();
 				cout << "輸入任意字符！繼續……";
 				getch();
 			}
 			else{
 				cout << "沒有找到該日期的資料！" << '\n' << endl;
 				cout << "輸入任意字符！繼續……";
 				getch();
 			}
 		}
 		break;
	}
}

//修改--------------------------------------------------------------//
void massage::ModifyItem(){
 	char name[20];
 	Money *p = NULL;
 	cout<<"請輸入要修改的理財名稱:";
	cin >> name;
 	if( p = FindItem( name )){
 		cout << "已找到該筆信息，請輸入新的信息!" << endl;
 		p -> Next -> Input();
 		cout << "已修改！" << endl;
 		cout << "輸入任意字符！繼續……";
 		getch();
 	}
 	else{
 		cout << "沒有找到!" << endl;
 		cout << "輸入任意字符！繼續……";
 		getch();
 	}
}
 
//刪除------------------------------------------------------// 
void massage::RemoveItem(){
 	char name[20];
 	Money *p = NULL , *temp = NULL;
 	cout << "請輸入要刪除的理財名稱:";
	cin >> name;
 	if( p = FindItem( name )){
 		temp = p -> Next;
 		p -> Next = p -> Next -> Next; 
 		delete temp;
 		cout << "已刪除！" << endl;
 		cout << "輸入任意字符！繼續……";
 		getch();
 	}
 	else{
 		cout << "沒有找到!" << endl;
  		cout << "輸入任意字符！繼續……";
 		getch();
 	}
}
  
//交換兩個combox---------------------------------------------------------------//
void massage::Swap( Money *p1, Money *p2){
	Money *temp = new Money; 
 	strcpy( temp -> name , p1 -> name );
 	strcpy( temp -> date , p1 -> date );
 	temp -> Inum = p1 -> Inum;
 	temp -> Onum = p1 -> Onum; 
 	temp -> sum = p1 -> sum;
 
 	strcpy(p1 -> name , p2 -> name );
 	strcpy(p1 -> date , p2 -> date );
 	p1 -> Inum = p2 -> Inum;
 	p1 -> Onum = p2 -> Onum;
 	p1 -> sum = p2 -> sum;
 
 	strcpy( p2 -> name , temp -> name) ;
 	strcpy( p2 -> date , temp -> date );
 	p2 -> Inum = temp -> Inum;
 	p2 -> Onum = temp -> Onum;
 	p2 -> sum = temp -> sum;
}
 
//統計-------------------------------------------------// 
int massage::ListCount(){
 	if(! Head)
 	return 0;
 	int n=0;
 	for(Money * p=Head->Next;p!=End;p=p->Next){
 		n++;
 	}
 	return n;
}
 
//排序﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌
void massage::Sort(){ 
 	cout << "Sorting..." << endl;
 	Money *p = NULL , *p1 = NULL, *k = NULL;
 	int n = massage::ListCount();
 	if( n < 2 ) 
 	return;
 	for( p = Head -> Next ; p != End; p = p -> Next ){
 		for( k = p -> Next; k != End; k = k -> Next ){
	 		if( p -> sum > k -> sum){
				massage::Swap(p,k);
	 		}
 		}
 	}
 	cout << "排序完成！" << endl;
 	getch();
 	return;
}
 
//﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌保存函數﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌
void massage::Save(){
 	out.open("sort.txt");
 	for( Money *p = Head -> Next ; p != End ; p = p -> Next )
 	out << p-> date << "\t\t" << p -> name << "\t\t" << p -> Inum << "\t\t" << p -> Onum << "\t\t" << p -> sum << '\n';
 	out.close();
}
 
//﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌主函數﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌﹌
int main(){
	int x , i = 0;
	bool quit = false;
	cout << "\t\t§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§" << endl;
	for( i = 0; i < 3; i++ )
		cout << "\t\t◎\t\t\t\t\t\t ◎" << endl;
		cout << "\t\t◎     ★★★★【 歡迎進入理財系統 】★★★★    ◎" << endl;
	for( i = 0; i < 3; i++ )
		cout << "\t\t◎\t\t\t\t\t\t ◎" << endl;
		cout << "\t\t§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§§\n" << endl;
		massage Grade;
		cout << "按任意鍵開始……";
		getch();
	while( !quit ){
		system("cls");
		Grade.ShowMenu();
		cin >> x;
		switch(x){
			case 0 : quit = true ;
				break;
			case 1 : Grade.AddItem() ; 
				break;
			case 2 : Grade.Display() ; 
				break;
			case 3 : Grade.Sort() ;
				break;
			case 4 : Grade.Find() ;
				break;
			case 5 : Grade.RemoveItem() ;
				break;
			case 6 : Grade.ModifyItem() ;
				break;
		}
	}
return 0;
} 
