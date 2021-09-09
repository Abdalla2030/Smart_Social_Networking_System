// Abdalla Fadl Shehata
// september , 2021
#include <iostream>
#include <cstdlib>
#include <string>
#include <fstream>
using namespace std;
//////////////////////////////////////////////
struct ListNode;
class TreeNode;
class FriendsBST;
class UserLL;
//////////////////////////////////////////////
class TreeNode
{
private:
    int priority;
    string username;
    ListNode *user;
    TreeNode *right;
    TreeNode *left;
public:
    TreeNode()
    {
        left = 0;
        right = 0;
    }
//////////////////////////////////////////////
    TreeNode(string name, ListNode*user_object, TreeNode*r = 0, TreeNode*l = 0)
    {
        username = name;
        user = user_object;
        right = r;
        left = l;
        priority = rand()%100;
    }
//////////////////////////////////////////////
    TreeNode* getLeft()
    {
        return left;
    }
    TreeNode* getRight()
    {
        return right;
    }
    string getUsername()
    {
        return username;
    }
    ListNode* getUser()
    {
        return user;
    }
    int getPriority(){
        return priority;
    }
//////////////////////////////////////////////
    void setLeft(TreeNode *l)
    {
        left = l;
    }
    void setRight(TreeNode *r)
    {
        right = r;
    }
    void setUsername(int name)
    {
        username = name;
    }
    void setUser(ListNode*user_object)
    {
        user = user_object;
    }
};
class FriendsBST {
    TreeNode *root;
public :
    FriendsBST()
    {
        root = 0;
    }
//////////////////////////////////////////////
void print()
{
         inOrder(root);
}
//////////////////////////////////////////////
void inOrder(TreeNode *node)
{
        if (node != 0){
            inOrder( node->getLeft() );
            cout << node->getUsername()<<" " ;
            inOrder( node->getRight() );
        }
}
//////////////////////////////////////////////
//left rotation
TreeNode *leftRotate(TreeNode *x)
{
        TreeNode *y = x->getRight(), *T2 = y->getLeft();
        // Perform rotation
        y->setLeft(x);
        x->setRight(T2);
        // Return new root
        return y;
}
//////////////////////////////////////////////
//right rotation
TreeNode *rightRotate(TreeNode *y)
{
        TreeNode *x = y->getLeft(),  *T2 = x->getRight();
        // Perform rotation
        x->setRight(y);
        y->setLeft(T2);
        // Return new root
        return x;
}
//////////////////////////////////////////////
void addUser(string name, ListNode*user_object){
        root = insert(root,name,user_object);
}
//////////////////////////////////////////////
TreeNode* insert(TreeNode*r, string name, ListNode*user_object)
{
        if (r == 0){
            return new TreeNode(name, user_object);
        }
         ////////////////////////////////////////////////////
        // insert right
        if (name > r->getUsername() ){
            r->setRight(insert(r->getRight(),name,user_object));
             // balancing
             if (r->getRight()->getPriority() > r->getPriority()){
                r = leftRotate(r);
           }
        }
        ///////////////////////////////////////////////////////
        // insert left
        else{
                 r->setLeft(insert(r->getLeft(),name,user_object));
                   // balancing
                 if (r->getLeft()->getPriority() > r->getPriority()){
                       r = rightRotate(r);
                 }
        }
        return r;
 }
////////////////////////////////////////////////
ListNode* Find(string name){
        TreeNode *temp1= search(root,name);
        if (temp1 != nullptr){
                ListNode *temp2 = temp1->getUser();
                 return temp2;
        }
        return nullptr;
}
//////////////////////////////////////////////
TreeNode* search(TreeNode *root, string name)
{
        if (root == 0){
            return nullptr ;
        }
        ///////////////////////////////////////////////
        if (name==root->getUsername()){
            return root;
        }
        //////////////////////////////////////////////
        else if ( root->getLeft() != 0 && name < root->getUsername()){
             return  search( root->getLeft(),name);
        }
        //////////////////////////////////////////////
        else if ( root->getRight() != 0 && name > root->getUsername()){
             return search(root->getRight(),name);
        }
        ////////////////////////////////////////////
        else {
            return nullptr;
        }
}
//////////////////////////////////////////////////////////////////
void deleteUser(string s)
{
	root = deleteNode(root, s);
}
/////////////////////////////////////////////////////////////////////
TreeNode* deleteNode(TreeNode* root, string name){
    if (root == 0){
        return root;
    }
    if (name < root->getUsername()){
        root->setLeft(deleteNode(root->getLeft(),name));
    }
    else if (name > root->getUsername()){
        root->setRight(deleteNode(root->getRight(),name));
    }
    // IF USER IS A ROOT
    //////////////////////////////////////////////////////
    // If left is NULL
    else if (root->getLeft() == 0)
    {
        TreeNode *temp = root->getRight();
        delete(root);
        root = temp;  // Make right child as root
    }
     //////////////////////////////////////////////////////
    // If right is NULL
    else if (root->getRight() == 0)
    {
        TreeNode *temp = root->getLeft();
        delete(root);
        root = temp;  // Make left child as root
    }
    // If user is a root and both left and right are not NULL
    else if (root->getLeft()->getPriority() < root->getRight()->getPriority())
    {
        root = leftRotate(root);
        root->setLeft(deleteNode(root->getLeft(),name));
    }
    else
    {
        root = rightRotate(root);
        root->setRight(deleteNode(root->getRight(), name));
    }
    return root;
   }
   /////////////////////////////////////////////
   ~FriendsBST(){
        Clear(root);
   }
   /////////////////////////////////////////////
   void Clear(TreeNode *root)
    {
        if(root != 0) {
            if( (root->getLeft() == 0) && (root->getRight() == 0) ) {
                delete root;
                root = 0;
            } else {
                Clear(root->getLeft());
                Clear(root->getRight());
            }
	}
	return;
    }
    /////////////////////////////////////////////
};
//////////////////////////////////////////////
struct ListNode
{
    string userName;
    string name;
    string email;
    FriendsBST friends;
    ListNode* next;
};
//////////////////////////////////////////////
class UserLL {
private:
    ListNode *head , *tail;
public:
    UserLL(){
        head=nullptr;
        tail=nullptr;
    }
    ListNode* begin (){
        return (this->head);
    }
    void push_back(const string& username , const string& name1 , const string& email1 )
    {
        ListNode *tmp=new ListNode;
        tmp->name=name1;
        tmp->userName=username;
        tmp->email=email1;
        tmp->next=nullptr;
        if(this->head==nullptr)
        {
            this->head=tmp;
            this->tail=tmp;
            tmp=nullptr;
        }
        else
        {
            this->tail->next=tmp;
            this->tail=tmp;
        }
    }
//////////////////////////////////////////////
ListNode *returnNode(string x){
    ListNode *n = head;
    while (n != nullptr)
    {
        if (n->userName == x){
            return n ;
        }
        n=n->next;
    }
    return nullptr;
}
//////////////////////////////////////////////
    void addUsers(fstream&file, const string&fileName)
    {
        string username,name,email;
        file.open(fileName, ios::in);
        if(!file) {
            cout<<"Error, File Not Found"<<endl;
        }
        file.seekg(0,ios::beg);
        while(!file.eof()){
            getline(file,username,',');
            getline(file,name,',');
            getline(file,email);
            this->push_back(username , name , email);
            file.peek();
        }
        file.close();
        //cout<<"File Opened"<<endl;
    }
//////////////////////////////////////////////
    void addFriends(fstream &file,const string &fileName){
        string name1,name2,temp;
        ListNode *temp_node;
        file.open(fileName, ios::in);
        if(!file) {
            cout<<"Error, File Not Found"<<endl;
        }
        file.seekg(0,ios::beg);
        while(!file.eof()){
            getline(file,name1,',');
            getline(file,temp);
            // remove first character (space) from second username
            for (int i =1;i<temp.size() ;i++){
                name2.push_back(temp[i]);
            }
            ///////////////////////////////////////////////
           // add left name as friend to right name
            ListNode *n = head ;
            while (n != nullptr)
            {
                if (n->userName == name2){
                    temp_node = n;
                    break;
                }
                else {
                    n=n->next;
                }
            }
            n = head ;
            while (n != nullptr)
            {
                if (n->userName == name1){
                    n->friends.addUser(name2,temp_node);
                    break;
                }
                else {
                    n=n->next;
                }
            }
            ////////////////////////////////////////////////
             // add right name as friend to left name
            n = head ;
            while (n != nullptr)
            {
                if (n->userName == name1){
                    temp_node = n;
                    break;
                }
                else {
                    n=n->next;
                }
            }
            n = head ;
            while (n != nullptr)
            {
                if (n->userName == name2){
                    n->friends.addUser(name1,temp_node);
                    break;
                }
                else {
                    n=n->next;
                }
            }
            ////////////////////////////////////////////////
            name2 = "";
            file.peek();
        }
        file.close();
        // cout<<"File Opened"<<endl;
    }
    //////////////////////////////////////////////////////////////
    void addFriend(string s1,string s2){
        if (s1== s2){
        cout<<"**********************************"<<endl;
        cout<<" You can't add yourself "<<endl;
        cout<<"**********************************"<<endl;
        return;
        }
        ListNode *tmp_node1 = returnNode(s1);
        ListNode *tmp_node2 = returnNode(s2);
        ListNode*tmp_node3 = tmp_node1->friends.Find(s2);
        if (tmp_node3 == nullptr ){
        tmp_node1->friends.addUser(s2,tmp_node2);
        tmp_node2->friends.addUser(s1,tmp_node1);
        cout<<"**********************************"<<endl;
        cout<<" You are now friends "<<endl;
        cout<<"**********************************"<<endl;
        }
        else {
            cout<<"**********************************"<<endl;
            cout<<"You are already friends"<<endl;
            cout<<"**********************************"<<endl;
        }
    }
    ///////////////////////////////////////////////////////////////
    void removeFriend(string s1,string s2){
        ListNode *tmp_node1 = returnNode(s1);
        ListNode *tmp_node2 = returnNode(s2);
        ListNode*tmp_node3 = tmp_node1->friends.Find(s2);
        if (tmp_node3 == nullptr){
        cout<<"**********************************"<<endl;
        cout<<"Friend not found "<<endl;
        cout<<"**********************************"<<endl;
        }
        else {
         tmp_node1->friends.deleteUser(s2);
         tmp_node2->friends.deleteUser(s1);
        cout<<"**********************************"<<endl;
        cout<<"Removed Done"<<endl;
        cout<<"**********************************"<<endl;
        }
    }
     //////////////////////////////////////////////////////////////
    ~UserLL(){

        while(head != tail){
            ListNode *cur=new ListNode;
            ListNode *pre=new ListNode;
            cur=head;
            while(cur->next != nullptr)
            {
                pre=cur;
                cur=cur->next;
            }
            delete cur;
            tail=pre;
            pre->next=nullptr;
        }
        delete head;
        delete tail;
    }
    ///////////////////////////////////////////////////
};
//////////////////////////////////////////////////////////////////////
void peopleYouMayKnow(string name,UserLL &l1){
         int index = 0 ;
         ListNode *people[5];
         ListNode *n1= l1.begin();
         ListNode* userNode = l1.returnNode(name);
         ///////////////////////////////////////////////
         while(index<5 && n1 != nullptr){
                if(n1->userName != userNode->userName &&  userNode->friends.Find(n1->userName) == nullptr){
                    people[index] = n1;
                    index++;
                }
                n1=n1->next;
         }
         /////////////////////////////////////////////
         if(index == 0){
              cout<<"**********************************"<<endl;
              cout<<"There is no match people for you"<<endl;
              cout<<"**********************************"<<endl;
         }
        else{
            cout<<"**********************************"<<endl;
            cout<<"The People You May Know"<<endl;
            for(int i=0;i<index;i++){
                cout<<(i+1)<<") "<<people[i]->userName<<", "<<people[i]->name<<endl;
            }
             cout<<"**********************************"<<endl;
    }
    ///////////////////////////////////////////////////////////////
    }
/////////////////////////////////////////////////////////
 void userMenu()
 {
     cout<<"1- List all friends "<<endl;
     cout<<"2- Search by username "<<endl;
     cout<<"3- Add friend  "<<endl;
     cout<<"4- Remove friend "<<endl;
     cout<<"5- People you may know "<<endl;
     cout<<"0- Log-out "<<endl;
 }
///////////////////////////////////////////////
int main() {
    UserLL l1 ;
    fstream  file;
    l1.addUsers(file,"all-users.in");
    l1.addFriends(file,"all-users-relations.in");
    ///////////////////////////////////////////////////////////////////////////
    int fChoice=1  ;
    while (fChoice){
    cout<<"1- Log in."<<endl;
    cout<<"0- Exit."<<endl;
    cin>>fChoice;
    if (fChoice==1){
        cout<<"Enter your user-name: ";
        string user_name;   cin>>user_name;
        ListNode *n1= l1.begin();   bool found= false;
        while (n1!=nullptr && !found)
        {
            if (user_name==n1->userName)
            {
                found= true;
            }
            n1=n1->next;
        }
        if (found){
            cout<<"welcome "<<user_name<<endl;
            int sChoice=1;
            while (sChoice){
                userMenu();
                cin>>sChoice;
                if (sChoice==1){
                            /////////////////////////////////////////////////////
                            ListNode* node = l1.returnNode(user_name);
                            cout<<"**********************************"<<endl;
                            node->friends.print();
                            cout<<endl;
                            cout<<"**********************************"<<endl;
                            cout<<endl;
                            /////////////////////////////////////////////////////
                }
                else if (sChoice==2){
                        //////////////////////////////////////////////
                        bool b = true;
                        string user_name_friend;
                        cout<<"**********************************"<<endl;
                         cout<<"Enter friend name : ";
                         cin>> user_name_friend;
                         ////////////////////////////////////////////////////////////
                        //check user_name_friend there are in linked list or not
                        ListNode *node = l1.returnNode(user_name_friend);
                        if (node==nullptr){
                            b = false ;
                            cout<<"**********************************"<<endl;
                            cout<<"friend name not found"<<endl;
                            cout<<"**********************************"<<endl;
                        }
                        if ( b == true){
                            ListNode *node1 = l1.returnNode(user_name);
                            ListNode *node2 = node1->friends.Find(user_name_friend);
                            if (node2 == nullptr){
                            cout<<"**********************************"<<endl;
                            cout<<"friend not found"<<endl;
                            cout<<"**********************************"<<endl;
                           }
                           else {
                            cout<<"**********************************"<<endl;
                            cout<<node2->userName<<","<<node2->name<<","<<node2->email<<endl;
                            cout<<"**********************************"<<endl;
                          }
                        }
                        /////////////////////////////////////////////////////////////
                }
                else if (sChoice==3){
                    ////////////////////////////////////////////////////////
                    string friend_name;
                    bool b = false ;
                    cout<<"**********************************"<<endl;
                    cout<<"Enter name of friend : ";
                    cin>> friend_name;
                    ListNode *node = l1.returnNode(friend_name);
                    if (node != nullptr){
                        b = true;
                    }
                        if (b){
                            l1.addFriend(user_name,friend_name);
                        }
                        else {
                            cout<<"**********************************"<<endl;
                            cout<<"friend name not found"<<endl;
                            cout<<"**********************************"<<endl;
                        }
                    ///////////////////////////////////////////////////////
                }
                else if (sChoice==4){
                    //////////////////////////////////////////////////////////////////
                    string friend_name;
                    bool b = false ;
                    cout<<"**********************************"<<endl;
                    cout<<"Enter name of friend : ";
                    cin>> friend_name;
                    ListNode *node = l1.returnNode(friend_name);
                    if (node != nullptr){
                        b = true;
                    }
                    if (b){
                            l1.removeFriend(user_name,friend_name);
                        }
                        else {
                            cout<<"**********************************"<<endl;
                            cout<<"friend name not found"<<endl;
                            cout<<"**********************************"<<endl;
                        }
                   ///////////////////////////////////////////////////////////////////
                }
                else if (sChoice==5){
                    peopleYouMayKnow(user_name,l1);
                }
                else if (sChoice==0){
                    user_name="";
                    cout<<"**********************************"<<endl;
                    cout<<"log-out successfully."<<endl;
                    cout<<"**********************************"<<endl;
                }
            }
        }
        else {
            cout<<"**********************************"<<endl;
            cout<<"user name is not found, please try to log-in again."<<endl;
            cout<<"**********************************"<<endl;
        }
    }
    else if (fChoice==0)
    {
        exit(0);
    }
    }
    return 0;
}
