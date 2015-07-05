/*
Solution to the Conference Track Management by Suraj Tripathi.
Algorithm explained in Algo.txt
*/
#include<iostream>
#include<fstream>
#include<string>

using namespace std;

int no_talks;		//no_talks keep count of number of talks
string line[1000];	//maintains the description of talks
int talks_time[1000];	//time required for each talk in talks_time
bool lightning;		//keep track that lightning present in talk or not.
float min_tracks;	//min_tracks hold the value for minumium tracks required by total_time_of_talks/420;
int totaltime_talks;
int talk_allocation[1000][2];	// maintain information about track and session of each talk
bool subset[1000][1000];	//array use to find complete subsets with sum either 180 or 240 minutes
int actual_count_talks;		// maintain duplicate total no. of talks
int sessions_reserved[1000][2];	
int time1[1000];
int max_track;			//holds the value of tracks required for all the talks


/*

remaining_talks() updates no_talks needed to be scheduled, after allocating time to some talks we call this function which updates no_talks which contains no. of talks needed to be scheduled and there corresponding time is present in talks_time.
As talks_time[] keeps on changing according to allocated talks, we maintained a intial copy of time required by talks in the array time1[].
*/
void remaining_talks(){

int temp[no_talks];
int j=0;

	for(int i=0;i<no_talks;i++){

				if(talk_allocation[i][0]!=0)
				i++;
				else{
					temp[j++]=talks_time[i];
					

				}

				}

	no_talks=j;			// updating no_talks left to schedule

		for(int i=0;i<no_talks;i++)
		talks_time[i]=temp[i];




}
/*

allocate() function is called when we found a subset of talks time either of sum 180 or 240, after finding sum we update the array talk_allocation which holds the track and session allocated to each talk.

*/
void allocate(bool subset[][1000],int n, int sum, int track_no, int session){
if(sum==0)
return;


if(subset[sum-talks_time[n-1]][n-1]==true){

//printf("%d ",set[n-1]);
talk_allocation[n-1][0]=track_no;
talk_allocation[n-1][1]=session;

allocate(subset,n-1,sum-talks_time[n-1],track_no,session);
}else{

allocate(subset,n-1,sum,track_no,session);

}


}

/*

findSubsetSum() function finds subset with either sum of 180 or 240 from the set containing time of all talks needed to be scheduled.
*/
bool findSubsetSum(int set[], int n, int sum, int track_no, int session)
{
    // The value of subset[i][j] will be true if there is a subset of set[0..j-1]
    //  with sum equal to i
//    bool subset[sum+1][20];
 
    // If sum is 0, then answer is true
    for (int i = 0; i <= n; i++)
      subset[0][i] = true;
 
    // If sum is not 0 and set is empty, then answer is false
    for (int i = 1; i <= sum; i++)
      subset[i][0] = false;
 
     // Fill the subset table in botton up manner
     for (int i = 1; i <= sum; i++)
     {
       for (int j = 1; j <= n; j++)
       {
         subset[i][j] = subset[i][j-1];
         if (i >= set[j-1])
           subset[i][j] = subset[i][j] || subset[i - set[j-1]][j-1];

       }
     }
 
    // uncomment this code to print table
    /* for (int i = 0; i <= sum; i++)
     {
       for (int j = 0; j <= n; j++)
          printf ("%4d", subset[i][j]);
       printf("\n");
     }*/

 
 	if(subset[sum][n]==true){
	allocate(subset,n,sum,track_no,session);
	sessions_reserved[track_no][session-1]=1;
     	}
}


int main(){

	ifstream myfile("input.txt");

	if(myfile.is_open()){
		
		while(getline(myfile,line[no_talks])){
									// reading from file. 
		//cout<< line[no_talks]<< '\n';
		no_talks++;

		}
		myfile.close();


	}else cout<< "unable to open file";


	actual_count_talks=no_talks;
	
	for(int i=0;i<no_talks;i++){
		int j=0;		//keep track of length of talk desc.
		lightning=false;
		while(line[i][j]!='\0')
		{
			if(line[i][j]>=48 && line[i][j]<58){
				talks_time[i]=line[i][j]-48;
				lightning=true;
				j++;
			
			}							// finding time of each talk
		
			while(line[i][j]>=48 && line[i][j]<58){
				talks_time[i]*=10;
				talks_time[i]+=line[i][j]-48;	
				j++;
			}
			j++;


		}
		
		if(lightning==false)
		talks_time[i]=5;
		
		}

/*// uncomment to print the extracted time of talks.
		for(int i=0;i<no_talks;i++)
		cout<<talks_time[i]<< '\n';
*/


		for(int i=0;i<no_talks;i++){

		time1[i]=talks_time[i];

		}
		for(int i=0;i<no_talks;i++){

		totaltime_talks+=talks_time[i];
		
		}

		min_tracks=(float)totaltime_talks/420;
		
	
		for(float i=0.0;i<=min_tracks;i=i+0.5){
		
			int temp=i;
			if(temp<i){
				findSubsetSum(talks_time, no_talks, 240, temp+1, 2);							

	
			}else{
			
				findSubsetSum(talks_time, no_talks, 180, temp+1, 1);	



			}
			remaining_talks();


		}


		/*for(int i=0;i<actual_count_talks;i++){
				
				cout<< talk_allocation[i][0]<<' '<<talk_allocation[i][1]<<'\n';



			}*/


			int i=1;
			while(no_talks!=0){
		
			
			while(sessions_reserved[i][0] != 0 && sessions_reserved[i][1] != 0)
			i++;
			
			if(sessions_reserved[i][0]==0){
				int temp=180;
				int k=0;
				for(int j=0;j<actual_count_talks;j++){
					if(talk_allocation[j][0]==0)
						{

							if(talks_time[k]<=temp){
					
								temp-=talks_time[k];
								talk_allocation[j][0]=i;
								talk_allocation[j][1]=1;
								cout<<'h'<<i<<'\n';
								k++;
								//cout<<talk_allocation[0][0]<<talk_allocation[0][1]<<'\n';
								
								}

						}
					

					}

				}else{

					int temp=240;
					int k=0;
					for(int j=0;j<actual_count_talks;j++){
						if(talk_allocation[j][0]==0)
							{

								if(talks_time[k]<=temp){
					
									temp-=talks_time[k];
									talk_allocation[j][0]=i;
									talk_allocation[j][1]=2;
									k++;
									}

							}
				

						}


				} 
			//cout<<no_talks<<'\n';
			remaining_talks();

			}		

	/*for(int i=0;i<actual_count_talks;i++){
				
				cout<< talk_allocation[i][0]<<' '<<talk_allocation[i][1]<<'\n';



			}
*/

// find maximum track 
	i=1;
	while(sessions_reserved[i][0] != 0 && sessions_reserved[i][1] != 0)
			i++;

	if(sessions_reserved[i][0] == 0 && sessions_reserved[i][1] == 0)
	{
	max_track=i-1;

	}
	else 
	max_track=i;


//printing result
	
	for(int l=1;l<=max_track;l++){
		cout<<"Track "<<l<<":"<<'\n';
		int hr=9;
		int min=0;
		for(int i=0;i<actual_count_talks;i++)
			if(talk_allocation[i][0]==l && talk_allocation[i][1]==1){
			cout<<hr<<":"<<min<<"AM "<<line[i]<<'\n';
			if(time1[i]>=60){
			int temp=time1[i]/60;
			min+=time1[i]-(temp*60);
			hr+=temp;	
			if(min>=60)
			{
			hr++;
			min=min-60;
			}	

			}else{
			min+=time1[i];
			if(min>=60)
			{
			hr++;
			min=min-60;
			}

			}
			}

		cout<<"12:00PM Lunch"<<'\n';
			hr=1;
			min=0;
		for(int i=0;i<actual_count_talks;i++)
			if(talk_allocation[i][0]==l && talk_allocation[i][1]==2){
			cout<<hr<<":"<<min<<"PM "<<line[i]<<'\n';
			if(time1[i]>=60){
			int temp=time1[i]/60;
			min+=time1[i]-(temp*60);
			hr+=temp;	
			if(min>=60)
			{
			hr++;
			min=min-60;
			}	

			}else{
			min+=time1[i];
			if(min>=60)
			{
			hr++;
			min=min-60;
			}

			}
			}
		
		cout<<"05:00PM Networking Event"<<'\n';
	


		}




return 0;
}
