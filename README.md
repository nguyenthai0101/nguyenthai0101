#include<iostream>
#include<string>
#include<fstream>
#include<iomanip>
#include<sstream>
#include<vector>
using namespace std;
typedef struct SinhVien{
	string MSSV,HoDem,Ten, Lop, GioiTinh;
	float diemTB;
	int ngay, thang, nam;
}SV;

/*kiem tra xem ten nhap vao co phai chi gom chu cai khong
neu chi gom chu cai thi tra ve true, neu khong tra ve false
*/
bool ktTT(string a){
	int n= a.size();
	for(int i=0;i<n;i++){
		if (a[i]>='a'&& a[i]<='z'||a[i]>='A'&& a[i]<='Z'){
			return true;
		}
		else
		return false;
	}	
}
// ham viet hoa chu cai dau
void vietHoa(string& a){
	int n=a.size();
	if(ktTT(a)){	// neu ten nhap vao chi toan chu cai thi hop le
		for(int i=0;i<n;i++){
		if(i==0){	//xet chu cai dau tai a[0] neu la chu thuong thi viet hoa
			if(a[i]>='a'&&a[i]<='z')
			a[i]=a[i]-32;
			else a[i]=a[i];
			}
		else{	
			
			if(a[i]>='a'&&a[i]<='z'&&a[i-1]==' ')	//neu a[i] viet thuong va truoc no có dau cach ==> viet hoa 
				a[i]=a[i]-32;
			else if(a[i]>='A'&&a[i]<='Z'&&(a[i-1]!=' '||i==n-1))//neu a[i] viet hoa nhung truoc no khong co dau cách ==> viet thuong
				a[i]=a[i]+32;
				// cac tuong hop con lai thi giu ngyen
			}
		}
	}
	else
	cout<<"\n Thong tin nhap vao khong hop le!";
}
//ham kiem tra ngay thang nam sinh
bool ktNgayThangNam(int ngay, int thang, int nam){ 
	if(nam<1923||nam>2023)
	return false;
	if(thang<0||thang>12)
	return false;
	switch(thang){
		case 2:
			if(ngay<1||ngay>29)
				return false;
			else if (ngay==29){
				if((nam%400==0)||(nam%4==0&&nam%100!=0))// xet nam nhuan
				break;
				else 
				return false;
			}
			break;
		case 4:
		case 6:
		case 9:
		case 11:
			if(ngay<1||ngay>30)
			return false;
			break;
		default:
			if(ngay<1||ngay>31)
			return false;
			break;
	}
	return true;	
}
// ham kiem tra ma so sinh vien, voi quy dinh chi bao gom so va max la 10 so
bool ktMSSV(string a){
	int n=a.size();
	if(n>10)
	return false;
	for(int i=0;i<n;i++){
		if(a[i]<'0'||a[i]>'9')
		return false;
	}
	return true;
}
void NhapHoDem(SV &a){
	cout << "Nhap ho dem sinh vien: ";
        do{
        	getline(cin,a.HoDem);
        	if(ktTT(a.HoDem)==false){
        	cout<<"Nhap sai moi ban nhap lai:";
			}	
		}while(ktTT(a.HoDem)==false);
		vietHoa(a.HoDem);
}
void NhapTen(SV &a){
	cout << "Nhap ten sinh vien: ";
        do{
        	getline(cin,a.Ten);
        	if(ktTT(a.Ten)==false){
        	cout<<"Nhap sai moi ban nhap lai:";
			}	
		}while(ktTT(a.Ten)==false);
		vietHoa(a.Ten);
}
void NhapLop(SV &a){
	cout << "Nhap lop cua sinh vien: ";
        cin >> a.Lop;
}
void NhapMSSV(SV &a){
	cout << "Nhap ma sinh vien: ";
        do
        {
            cin >> a.MSSV;
            if(ktMSSV(a.MSSV)==false)
                cout << "Nhap sai moi ban nhap lai: ";
        }
        while (ktMSSV(a.MSSV) == false);
}
void NhapNTN(SV &a){
	bool kt=false;
        do{
        cout<<"Nhap ngay thang nam sinh cua sinh vien: ";
        cin>>a.ngay>>a.thang>>a.nam;
         kt=cin.fail()||!ktNgayThangNam(a.ngay,a.thang,a.nam);
        if(kt){
        	cout<<"du lieu vao khong hop le!"<<endl;
        	cin.clear();
        	cin.ignore();	
				}
        else{
        	cout<<"du lieu hop le"<<endl;
        	break;
		}
		
		
    }while(kt);
}
void NhapGT( SV &a){
	cout << "Nhap gioi tinh cua sinh vien: ";
        do
        {
        	cin.ignore();
        	cin>>a.GioiTinh;
            if(ktTT(a.GioiTinh)==false)
            {
                cout << "Nhap sai moi ban nhap lai: ";
            }
        } while(ktTT(a.GioiTinh)==false);
        	vietHoa(a.GioiTinh); 
}
void NhapDiemTB(SV &a){
	bool kt=false;
	 do
        {
            cout << "Nhap diem trung binh: ";
            cin >> a.diemTB;
            kt=cin.fail();
            if(kt){
            	cout<<"Diem nhap vao khong hop le, moi nhap lai!"<<endl;
            	cin.clear();
            	cin.ignore();
			}
        }
        while(a.diemTB<0||a.diemTB>10||kt);
}
// ham nhap thong tin
void Nhap(SV &a)
    {	//nhap ho dem
        NhapHoDem(a);
		//nhap ten
		NhapTen(a);
		//nhap lop
        NhapLop(a);
        //nhap ma sinh vien
        NhapMSSV(a);
        //nhap ngay thang nam sinh
       NhapNTN(a);
    	//nhap gioi tinh
        NhapGT(a);
        //nhap diem trung binh
        NhapDiemTB(a);
}
//ham nay cho phep nhap 1 luc nhieu sv
void NhapSv(vector<SV>&a){
	int k=a.size();
	int i;
	bool kt=false;	//bien kiem tra thong tin so i nhap vao
	SV sv;
	do
    {
        cout << "Nhap so luong sinh vien: ";
        cin >> i;
        if(i==0){
        	cout<<"So luong nhap vao khong hop le, moi nhap lai"<<endl;
		}
        kt=cin.fail();	 //neu kt=true thi du lieu nhap vao khong phai so nguyen
        if(kt)	// du lieu nhap vao khong phai so nguyen thi cho nhap lai
        cout << "Moi ban nhap lai!" << endl;
        cin.clear();
        cin.ignore();
    }while(kt||i==0);// thuc hien nhap den khi nao duoc gia tri hop le
    int n=k+i;	//tang so luong phan tu cua mang sv
    bool p=false;
    for (int j = k; j<n; j++)
    {
    	cout<<"==========================="<<endl;
        cout <<"Nhap thong tin sinh vien thu " << j+1 << ":" << endl;
		Nhap(sv);
		if(j>=1){
		do{
        for(int b=0;b<j;b++){
        	if(sv.MSSV==a[b].MSSV){
        	cout<<"MSSV da ton tai, moi nhap lai MSSV"<<endl;
        	NhapMSSV(sv);
        }
        else p=true;
		}
	}while(!p);
}
	a.push_back(sv);
        cin.ignore();
	}
	return;
}
void Xuat(SV a){
	string b=a.HoDem+" "+a.Ten;
	stringstream ss1;
	ss1 <<a.ngay;
    string ngay= ss1.str();
    stringstream ss2;
	ss2 <<a.thang;
    string thang= ss2.str();
    stringstream ss3;
	ss3 <<a.nam;
    string nam= ss3.str();
    string c= ngay+"/"+thang+"/"+nam;
	cout<<left<<setw(25)<<b<<setw(15)<<a.Lop<<setw(15)<<a.MSSV<<setw(15);
	cout<<c<<setw(15)<<a.GioiTinh<<setw(15)<<a.diemTB <<endl;
}

//ham xuat danh sach sinh vien
void XuatSv(vector<SV>a){
	cout<<"==========================================Danh sach sinh vien=========================================="<<endl;
	cout<<"\t"<<left<<setw(10)<<"STT"<<setw(25)<<"Ho ten"<<setw(15)<<"Lop"<<setw(15)<<"MSSV"<<setw(15);
	cout<<"Ngay sinh"<<setw(15)<<"Gioi tinh"<<setw(15)<<"Diem TB"<<endl;
	for(int i=0;i<a.size();i++){
		cout<<"\t"<<left<<setw(10)<<i+1;
		Xuat(a[i]);
	}
}

// them sinh vien
void Them(vector<SV>a){// n la so sv da nhap
	int n=a.size();
	n++;
	bool kiemtra;
	do{
		cout<<"Nhap thong tin sinh vien ma ban muon them:"<<endl;
		Nhap(a[n-1]);
		kiemtra=true;
		for(int i=0;i<n-1;i++){
			if(a[i].MSSV==a[n-1].MSSV){
				cout<<"MSSV ban vua nhap da ton tai, moi nhap lai"<<endl;
			kiemtra=false;
		}
		}
	}while(kiemtra!=true);
}
// xoa sinh vien theo mssv
void Xoa(vector<SV>a){
	int n=a.size();
	string mssv;
	do{
	cout<<"nhap vao MSSV can xoa: ";
	cin>>mssv;
	if(ktMSSV(mssv)==false){
	cout<<"Nhap sai moi ban nhap lai!"<<endl;;
	}
	}while (ktMSSV(mssv)==false);
	for(int i=0;i<n;i++){
		if(a[i].MSSV==mssv){
			for(int j=i;j<n-1;j++){
				a[j]=a[j+1];
			}
			n--;
			cout<<"da xoa thanh cong"<<endl;;
			break;
		}
		else
		cout<<"MSSV ban vua nhap khong ton tai"<<endl;
}
}
void MenuChinhSua()
{
        cout << ":===================Chinh sua===================:"<<endl;
        cout << "| 1. Ho ten                                     |"<<endl;
        cout << "| 2. Lop                                        |"<<endl;
        cout << "| 3. MSSV                                       |"<<endl;
        cout << "| 4. Gioi tinh                                  |"<<endl;
        cout << "| 5. Diem                                       |"<<endl;
        cout << "| 6. Ngay thang nam sinh                        |"<<endl;
        cout << ":===============================================:"<<endl;
}
// chinh sua thong tin theo mssv
void ChinhSua(vector<SV>a){
	int n=a.size();
	string mssv;
	do{
	cout<<"nhap vao MSSV can chinh sua thong tin: ";
	cin>>mssv;
	if(ktMSSV(mssv)==false)
	cout<<"Nhap sai moi ban nhap lai!";
	}while (ktMSSV(mssv)==false);
	for(int i=0;i<n;i++){
		if(a[i].MSSV==mssv){
		char chon;
		int luaChon;
		bool kt=false;
		do {
			MenuChinhSua();
		do
		{
		cout<<"Nhap vao lua chon: ";
		cin>>luaChon;
        kt=cin.fail();	 //neu kt=true thi du lieu nhap vao khong phai so nguyen
        if(kt)	// du lieu nhap vao khong phai so nguyen thi cho nhap lai
        cout << "Moi ban nhap lai voi du lieu la so nguyen tu 1 den 6!" << endl;
        cin.clear();
        cin.ignore();
    }while(kt);
		switch(luaChon){
			case 1:
				NhapHoDem(a[i]);
				NhapTen(a[i]);
			break;	
			case 2:
				NhapLop(a[i]);
				break;
			case 3:
				NhapMSSV(a[i]);
				break;
			case 4:
				NhapGT(a[i]);
				break;
			case 5:
				NhapDiemTB(a[i]);
				break;
			case 6:
				NhapNTN(a[i]);
				break;
		default:
           cout << "Ban da nhap sai lua chon!" << endl;
        break;
       }
       cout<<"==========================="<<endl;
       cout << "Ban muon lua chon tiep khong: "<<endl;
       bool h=false;
       do
		{
			cout<<"Nhap 'Y' hoac 'N' "<<endl;
       cin>>chon;
        h=cin.fail();	 
        if(h)
        cout<<"Du lieu nhap vao sai, moi nhap lai!"<<endl;
        cin.clear();
        cin.ignore();
    }while(h||chon!='Y'&& chon!='N');
       if(chon == 'N')
        cout << "Thoat khoi chuc nang sua" << endl;
	}while(chon=='Y');
			cout<<"DA CHINH SUA THONG TIN THANH CONG"<<endl;
			return;
		}
	}
		cout<<"MSSV ban vua nhap khong ton tai"<<endl;
	return;
}
// tim kiem sinh vien theo ten 
void tkTen(vector<SV>a){
	string ten;
	 cout<<"Nhap vao ten ban can tim kiem:"<<endl;
	do{
		getline(cin,ten);
		if(ktTT(ten)==false)
		cout<<"Du lieu nhap vao khong dung, moi nhap lai"<<endl;
	}while (ktTT(ten)==false);
	vietHoa(ten);
	bool kt=false;
	for(int i=0;i<a.size();i++){
		if(a[i].Ten==ten){
			cout<<"Ten ban tim kiem co ton tai"<<endl;
			cout<<"\t"<<left<<setw(10)<<"STT"<<setw(25)<<"Ho ten"<<setw(15)<<"Lop"<<setw(15)<<"MSSV"<<setw(15);
	cout<<"Ngay sinh"<<setw(15)<<"Gioi tinh"<<setw(15)<<"Diem TB"<<endl;
		cout<<"\t"<<left<<setw(10)<<i+1;
			Xuat(a[i]);
			kt=true;
		
	}
}
	if(!kt)
		cout<<"Ten ban tim kiem khong ton tai!"<<endl;

}
// tim kiem theo mssv
void tkMSSV(vector<SV>a){
	string mssv;
	cout<<"Nhap vao MSSV can tim kiem: ";
	do{
		cin>>mssv;
		if(ktMSSV(mssv)==false )
		cout<<"Ma nhap vao khong hop le, moi nhap lai"<<endl;
	}while(ktMSSV(mssv)==false);
	bool kt=false;
	for(int i=0;i<a.size();i++){
		if(a[i].MSSV==mssv){
			cout<<"MSSV ban tim kiem co ton tai"<<endl;
			cout<<"\t"<<left<<setw(10)<<"STT"<<setw(25)<<"Ho ten"<<setw(15)<<"Lop"<<setw(15)<<"MSSV"<<setw(15);
	cout<<"Ngay sinh"<<setw(15)<<"Gioi tinh"<<setw(15)<<"Diem TB"<<endl;
		cout<<"\t"<<left<<setw(10)<<i+1;
			Xuat(a[i]);
			kt=true;
		
	}
}
	if(!kt)
		cout<<"MSSV ban tim kiem khong ton tai!"<<endl;
}
// sap xep theo ten 
void SapXepTen(vector<SV>a){
	int n=a.size();
	for(int i=0;i<n-1;i++){
		for(int j=0;j<n-i-1;j++){
			if(a[j].Ten>a[j+1].Ten){
				SV temp=a[j];
				a[j]=a[j+1];
				a[j+1]=temp;
			}
		}
	}
}
// sap xep theo diem giam dan
void SapXepDiem(vector<SV>a){
	int n=a.size();
	for(int i=0;i<n-1;i++){
		for(int j=0;j<n-i-1;j++){
			if(a[j].diemTB<a[j+1].diemTB){
				SV temp=a[j];
				a[j]=a[j+1];
				a[j+1]=temp;
			}
		}
	}
}
void Menu()
{
        cout << "=========================QUAN LI SINH VIEN========================:"<<endl;
        cout << " 1. Nhap danh sach cua sinh vien                                  |"<<endl;
        cout << " 2. Hien thi danh sach cua sinh vien                              |"<<endl;
        cout << " 3. Them thong tin 1 sinh vien                                    |"<<endl;
        cout << " 4. Xoa thong tin 1 sinh vien                                     |"<<endl;
        cout << " 5. Chinh sua thong tin theo MSSV                                 |"<<endl;
        cout << " 6. Tim sinh vien bang ma so sinh vien                            |"<<endl;
        cout << " 7. Tim sinh vien bang ten sinh vien                              |"<<endl;
        cout << " 8. Sap xep sinh vien theo diem                                   |"<<endl;
        cout << " 9. Sap xep sinh vien theo ten                                    |"<<endl;
        cout << " 10.                                                              |"<<endl;
        cout << "==================================================================:"<<endl;
}

int main(){
	vector<SV>a;
	char chon;
	int luaChon;
	int n=0;
	bool kt=false;
	do {
		Menu();
		do
		{
		cout<<"Nhap vao lua chon: ";
		cin>>luaChon;
        kt=cin.fail();	 //neu kt=true thi du lieu nhap vao khong phai so nguyen
        if(kt)	// du lieu nhap vao khong phai so nguyen thi cho nhap lai
        cout << "Moi ban nhap lai voi du lieu la so nguyen tu 1 den 10!" << endl;
        cin.clear();
        cin.ignore();
    }while(kt);
		switch(luaChon){
			case 1:
				NhapSv(a);
			break;	
			case 2:
				XuatSv(a);
				break;
			case 3:
				Them(a);
				break;
			case 4:
				Xoa(a);
				break;
			case 5:
				ChinhSua(a);
				break;
			case 6://tim kiem bang ma
				tkMSSV(a);
			break;
			case 7:
				tkTen(a);
				break;
				// tim bang ten
			case 8://sap xep theo diem
				SapXepDiem(a);
				break;
			case 9://sap xep theo ten
				SapXepTen(a);
				break;
			case 10: 
			break;
		default:
           cout << "Ban da nhap sai lua chon!" << endl;
        break;
       }
       cout<<"==========================="<<endl;
       cout << "Ban muon lua chon tiep khong: "<<endl;
       bool a=false;
       do
		{
			cout<<"Nhap 'y' hoac 'n' "<<endl;
       cin>>chon;
        a=cin.fail();	 
        if(a)
        cout<<"Du lieu nhap vao sai, moi nhap lai!"<<endl;
        cin.clear();
        cin.ignore();
    }while(a||chon!='y'&& chon!='n');
       if(chon == 'n')
        cout << "Tam biet va hen gap lai" << endl;
	}
	while(chon=='y');
	return 0;
}
