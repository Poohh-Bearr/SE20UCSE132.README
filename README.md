# SE20UCSE132.README
#include<iostream>
#include<vector>
using namespace std;
#define vl vector<int>
#define vll vector<vl>
const int res = 100, proc = 100;
vll getans;
vll max_processes(proc, vl(res));
vll given(proc, vl(res));
vll need(proc, vl(res));
vl given_resources(res);
vl safe;
bool marked[proc] = {};
int R, P, available[res];

bool is_available(int process_id, vll &given, vll& max_processes, vll& need, int available[]){
	bool flag = true;
	for (int i = 0; i < R; i++) {
		if (need[process_id][i] > available[i])
			flag = false;
	}
	return flag;
}

void safe_sequence(bool marked[], vll& given, vll& max_processes,vll& need, int available[], vl safe)
{
	for (int i = 0; i < P; i++) {
		if (!marked[i] && is_available(i, given, max_processes, need, available)) {
			marked[i] = true;
			for (int j = 0; j < R; j++)
				available[j] += given[i][j];
			safe.push_back(i);
			safe_sequence( marked, given, max_processes, need, available, safe);
			safe.pop_back();
			marked[i] = false;
			for (int j = 0; j < R; j++)
				available[j] -= given[i][j];
		}
	}
	if (safe.size() == (unsigned long)P) {
        getans.push_back(safe);
	}
}
void print(){
    cout<< getans.size()<<"\n";
    for(auto i: getans){
        for(auto j: i){
            cout<<j<<" ";
        }
        cout<<"\n";
    }
}
void inp(){
    cin>>R>>P;   
    for(int i=0; i<R; i++)
        cin>>given_resources[i];
    for(int i=0; i<P; i++)
        for(int j=0; j<R; j++)
            cin>>max_processes[i][j];
    for(int i=0; i<P; i++)
        for(int j=0; j<R; j++)
            cin>>given[i][j];
}

void preCal(){
    for (int i = 0; i < R; i++) {
		int sum = 0;
		for (int j = 0; j < P; j++)
			sum += given[j][i];
		available[i] = given_resources[i] - sum;
	}
    for (int i = 0; i < P; i++)
		for (int j = 0; j < R; j++)
			need[i][j] = max_processes[i][j] - given[i][j];
}
int main(){
    inp();
	preCal();
	safe_sequence(marked, given, max_processes, need, available, safe);
    print();
}
