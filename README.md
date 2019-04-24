# banker-s-algorithm
Consider following and Generate a solution to find whether the system is in safe state or not?
// BANKER ALGORITHM

#include<iostream> 
using namespace std; 
const int P = 5; 

const int R = 4; 

void CalNeed(int need[P][R], int max[P][R], 
				int allot[P][R]) 
{ 
	// Need of each P 
	for (int i = 0 ; i < P ; i++) 
		for (int j = 0 ; j < R ; j++) 

			need[i][j] = max[i][j] - allot[i][j]; 
} 
//safe state or not
bool IsSafe(int processes[], int avail[], int max[][R], 
			int allot[][R]) 
{ 
	int need[P][R]; 

	//calculate need matrix 
	CalNeed(need, max, allot); 

	bool finish[P] = {0}; 

	int SafeSequence[P]; 

	int work[R]; 
	for (int i = 0; i < R ; i++) 
		work[i] = avail[i]; 

	int count = 0; 
	while (count < P) 
	{ 
		bool found = false; 
		for (int p = 0; p < P; p++) 
		{ 
			if (finish[p] == 0) 
			{ 
				int j; 
				for (j = 0; j < R; j++) 
					if (need[p][j] > work[j]) 
						break; 

				if (j == R) 
				{ 
					for (int k = 0 ; k < R ; k++) 
						work[k] += allot[p][k]; 

					SafeSequence[count++] = p; 

					finish[p] = 1; 

					found = true; 
				} 
			} 
		} 

		if (found == false) 
		{ 
			cout << "The System is not in safe state"; 
			return false; 
		} 
	} 

	cout << " The System is in safe state. and \n The Safe sequence is: ";  //safe sequence or not
	for (int i = 0; i < P ; i++) 
		cout << SafeSequence[i] << " "; 

	return true; 
} 

int main() 
{ 
	int processes[] = {0, 1, 2, 3, 4}; 
	int avail[] = {1,5,2,0}; 

	int max[][R] = {{0,0,1,2},    //max
					{1,7,5,0}, 
					{2,3,5,6}, 
					{0,6,5,2}, 
					{0,6,5,6}}; 

	int allot[][R] = {{0,0,1,2},  //allocation
					{1,0,0,0}, 
					{1,3,5,4}, 
					{0,6,3,2}, 
					{0,0,1,4}}; 

	IsSafe(processes, avail, max, allot); 

	return 0; 
}
