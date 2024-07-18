MongoDB Task

Designed a database as Zen class programme and created the following collections:
  - users
  -codekata
  -attendance
  -topics
  -tasks
  -company_drives
  -mentors

Following are the Queries for the corresponding questions:
1. Find all the topics and tasks which are thought in the month of October.
        Query: db.topics.aggregate([{
                $lookup:
                {from:'tasks',
                	localField:'_id',
                	foreignField:'topic_id',
                	as:'taskDetails'
                }},{$match:{ $and: [ { date: { $gte: new Date("2020-10-15")}},{ date:{$lte: new Date("2020-10-30") }} ]}
                },{ $project : { _id : 0 } }])
 
2. Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020.
        Query:  db.company_drives.find({ $and: [ { date: { $gte: new Date("2020-10-15")}},{ date:{$lte: new Date("2020-10-30") }} ]},{_id:0,selected:0})

3. Find all the company drives and students who are appeared for the placement.
       Query: db.company_drives.find({},{_id:0,date:0})
   
4. Find the number of problems solved by the user in codekata.
       Query: db.codekata.find({},{_id:0})
   
5. Find all the mentors with who has the mentee's count more than 15.
       Query: db.mentors.find({mentee_count:{$gt:15}})
   
6. Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020.
        Query: db.attendance.aggregate(
                  [
                    {
                      $lookup: {
                        from: 'tasks',
                        localField: 'topic_id',
                        foreignField: 'topic_id',
                        as: 'task_details'
                      }
                    },
                { $unwind: '$task_details' },
                {  $match: {
                        'task_details.due_date': {
                          $gte: '2020-10-15',
                          $lte: '2020-10-31'
                        }
                }},
                {
                $project:
                  {_id:0,topic_id:1,absent:1,'task_details.due_date':1,'task_details.submitted_users':1}
                
                }
                
                ])

      Collections, Queries and the corresponding outputs are submitted in a word file.


