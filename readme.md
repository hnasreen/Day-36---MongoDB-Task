### Design database for Zen class programme

**users**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f2" },
      "name": "John Doe",
      "email": "john.doe@example.com",
      "mentor_id": { "$oid": "60a2c6b4b6f6d9b0567763e1" }
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f3" },
      "name": "Jane Smith",
      "email": "jane.smith@example.com",
      "mentor_id": { "$oid": "60a2c6b4b6f6d9b0567763e2" }
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f4" },
      "name": "Mike Johnson",
      "email": "mike.johnson@example.com",
      "mentor_id": { "$oid": "60a2c6b4b6f6d9b0567763e1" }
   }
]

**codekata**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f5" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f2" },
      "problems_solved": 50
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f6" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f3" },
      "problems_solved": 30
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f7" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f4" },
      "problems_solved": 40
   }
]

**attendance**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f8" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f2" },
      "date": { "$date": "20-10-2020" },
      "status": "absent"
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763f9" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f3" },
      "date": { "$date": "21-10-2020" },
      "status": "present"
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763fa" },
      "user_id": { "$oid": "60a2c6b4b6f6d9b0567763f4" },
      "date": { "$date": "22-10-2020" },
      "status": "absent"
   }
]

**topics**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763fb" },
      "title": "MongoDB Basics",
      "date": { "$date": "15-10-2020" },
      "month": 10
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763fc" },
      "title": "Advanced MongoDB",
      "date": { "$date": "20-10-2020" },
      "month": 10
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763fd" },
      "title": "MongoDB Aggregation",
      "date": { "$date": "25-10-2020" },
      "month": 10
   }
]

**tasks**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763fe" },
      "topic_id": { "$oid": "60a2c6b4b6f6d9b0567763fb" },
      "title": "Complete MongoDB Tutorial",
      "due_date": { "$date": "18-10-2020" },
      "submitted": true
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763ff" },
      "topic_id": { "$oid": "60a2c6b4b6f6d9b0567763fc" },
      "title": "MongoDB Aggregation Framework",
      "due_date": { "$date": "25-10-2020" },
      "submitted": false
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b056776400" },
      "topic_id": { "$oid": "60a2c6b4b6f6d9b0567763fd" },
      "title": "MongoDB Performance Tuning",
      "due_date": { "$date": "30-10-2020" },
      "submitted": false
   }
]

**company_drives**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b056776401" },
      "company_name": "TechCorp",
      "date": { "$date": "16-10-2020" },
      "students_appeared": [
         { "$oid": "60a2c6b4b6f6d9b0567763f2" },
         { "$oid": "60a2c6b4b6f6d9b0567763f3" }
      ]
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b056776402" },
      "company_name": "InnoTech",
      "date": { "$date": "28-10-2020" },
      "students_appeared": [
         { "$oid": "60a2c6b4b6f6d9b0567763f3" },
         { "$oid": "60a2c6b4b6f6d9b0567763f4" }
      ]
   }
]

**mentors**

[
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763e1" },
      "name": "Alice Johnson",
      "mentee_count": 20
   },
   {
      "_id": { "$oid": "60a2c6b4b6f6d9b0567763e2" },
      "name": "Bob Brown",
      "mentee_count": 10
   }
]


**Find all the topics and tasks which are thought in the month of October**

db.topics.aggregate([
   {
      $match: {
         month: 10
      }
   },
   {
      $lookup: {
         from: "tasks",
         localField: "_id",
         foreignField: "topic_id",
         as: "tasks"
      }
   }
]);

**Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020**

db.company_drives.find({
   date: {
      $gte: ISODate("2020-10-15T00:00:00Z"),
      $lte: ISODate("2020-10-31T23:59:59Z")
   }
});

**Find all the company drives and students who are appeared for the placement.**

db.company_drives.aggregate([
   {
      $lookup: {
         from: "users",
         localField: "students_appeared",
         foreignField: "_id",
         as: "students"
      }
   }
]);

**Find the number of problems solved by the user in codekata**

db.codekata.aggregate([
   {
      $group: {
         _id: "$user_id",
         totalProblemsSolved: { $sum: "$problems_solved" }
      }
   }
]);

**Find all the mentors with who has the mentee's count more than 15**

db.mentors.find({
   mentee_count: { $gt: 15 }
});

**Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020**

db.attendance.aggregate([
   {
      $match: {
         status: "absent",
         date: {
            $gte: new ISODate("2020-10-15T00:00:00Z"),
            $lte: new ISODate("2020-10-31T23:59:59Z")
         }
      }
   },
   {
      $lookup: {
         from: "users",
         localField: "user_id",
         foreignField: "_id",
         as: "user"
      }
   },
   {
      $unwind: "$user"
   },
   {
      $lookup: {
         from: "codekata",
         localField: "user._id",
         foreignField: "user_id",
         as: "codekata"
      }
   },
   {
      $unwind: "$codekata"
   },
   {
      $lookup: {
         from: "tasks",
         let: { userId: "$user._id" },
         pipeline: [
            {
               $match: {
                  $expr: {
                     $and: [
                        { $eq: ["$submitted", false] },
                        { $gte: ["$due_date", new ISODate("2020-10-15T00:00:00Z")] },
                        { $lte: ["$due_date", new ISODate("2020-10-31T23:59:59Z")] }
                     ]
                  }
               }
            }
         ],
         as: "tasks"
      }
   },
   {
      $unwind: "$tasks"
   },
   {
      $group: {
         _id: "$user._id"
      }
   },
   {
      $count: "No_of_Students_Absent"
   }
]);