using System;
using System.Collections.Generic;

namespace AssignmentTracker
{
    class Program
    {
        static string[] courses = new string[0];
        
        static List<string> assignments = new List<string>();
        
        static Dictionary<string, string> assignmentDetails = new Dictionary<string, string>();

        static void Main(string[] args)
        {
            bool running = true;

            while (running)
            {
                DisplayMenu();
                string choice = Console.ReadLine();
                Console.WriteLine();

                if (choice == "1") CreateCourses();
                else if (choice == "2") UpdateCourse();
                else if (choice == "3") AddAssignments();
                else if (choice == "4") UpdateAssignment();
                else if (choice == "5") RemoveAssignmentByIndex();
                else if (choice == "6") AddOrUpdateDetails();
                else if (choice == "7") RemoveAssignmentByName();
                else if (choice == "8")
                {
                    running = false;
                    Console.WriteLine("Good work!");
                }
                else Console.WriteLine("Invalid option. Choose 1-8.");

                Console.WriteLine();
            }
        }

        // ---------- Menu ----------

        static void DisplayMenu()
        {
            Console.WriteLine("===== Assignment Tracker =====");
            Console.WriteLine("1. Create Course (Array)");
            Console.WriteLine("2. Update Course (Array)");
            Console.WriteLine("3. Add Assignment (List)");
            Console.WriteLine("4. Update Assignment (List)");
            Console.WriteLine("5. Remove Assignment by Index (List)");
            Console.WriteLine("6. Add or Update Assignment Details (Dictionary)");
            Console.WriteLine("7. Remove Assignment by Name (List && Dictionary)");
            Console.WriteLine("8. Exit");
            Console.Write("Choose an option: ");
        }

        // ---------- FR-01: Create Course Array ----------

        static void CreateCourses()
        {
            Console.Write("How many courses? ");
            int numberOfCourses;
            if (!int.TryParse(Console.ReadLine(), out numberOfCourses) || numberOfCourses < 0)
            {
                Console.WriteLine("Please enter a valid non-negative whole number.");
                return;
            }

            courses = new string[numberOfCourses];

            for (int i = 0; i < courses.Length; i++)
            {
                Console.Write("Enter course name for index " + i + ": ");
                courses[i] = Console.ReadLine();
            }

            Console.WriteLine();
            Console.WriteLine("Courses:");
            DisplayCourses();
        }

        // ---------- FR-02: Update Course ----------

        static void UpdateCourse()
        {
            if (courses.Length == 0)
            {
                Console.WriteLine("The course array has not been created. Use Option 1 first.");
                return;
            }

            Console.WriteLine("Current courses:");
            DisplayCourses();

            Console.Write("Enter the index of the course to update: ");
            int courseIndex;
            if (!int.TryParse(Console.ReadLine(), out courseIndex) ||
                !(courseIndex >= 0 && courseIndex < courses.Length))
            {
                Console.WriteLine("Invalid course index.");
                return;
            }

            Console.Write("Enter the new course name: ");
            courses[courseIndex] = Console.ReadLine();

            Console.WriteLine();
            Console.WriteLine("Updated courses:");
            DisplayCourses();
        }

        // ---------- FR-03: Add Assignments ----------

        static void AddAssignments()
        {
            Console.WriteLine("Current assignments:");
            DisplayAssignments();

            string response = "y";
            while (response == "y")
            {
                Console.Write("Enter assignment name: ");
                assignments.Add(Console.ReadLine());

                Console.Write("Add another assignment? (y/n): ");
                response = (Console.ReadLine() ?? "").Trim().ToLower();
            }

            Console.WriteLine();
            Console.WriteLine("Assignments:");
            DisplayAssignments();
        }

        // ---------- FR-04: Update Assignment ----------

        static void UpdateAssignment()
        {
            Console.WriteLine("Current assignments:");
            DisplayAssignments();

            Console.Write("Enter the index of the assignment to update: ");
            int assignmentIndex;
            if (!int.TryParse(Console.ReadLine(), out assignmentIndex) ||
                !(assignmentIndex >= 0 && assignmentIndex < assignments.Count))
            {
                Console.WriteLine("Invalid assignment index.");
                return;
            }

            string oldName = assignments[assignmentIndex];

            Console.Write("Enter the new assignment name: ");
            string newName = Console.ReadLine();

            assignments[assignmentIndex] = newName;
            
            if (assignmentDetails.ContainsKey(oldName))
            {
                string details = assignmentDetails[oldName];
                assignmentDetails.Remove(oldName);
                assignmentDetails[newName] = details;
                Console.WriteLine("Assignment details moved to the new name.");
            }

            Console.WriteLine();
            Console.WriteLine("Updated assignments:");
            DisplayAssignments();
        }

        // ---------- FR-05: Remove by Assignment Index ----------

        static void RemoveAssignmentByIndex()
        {
            Console.WriteLine("Current assignments:");
            DisplayAssignments();

            Console.Write("Enter the index of the assignment to remove: ");
            int removeIndex;
            if (!int.TryParse(Console.ReadLine(), out removeIndex) ||
                !(removeIndex >= 0 && removeIndex < assignments.Count))
            {
                Console.WriteLine("Invalid assignment index.");
                return;
            }

            assignments.RemoveAt(removeIndex);

            Console.WriteLine();
            Console.WriteLine("Updated assignments:");
            DisplayAssignments();
        }

        // ---------- FR-06: Add/Update Assignment Details ----------

        static void AddOrUpdateDetails()
        {
            Console.WriteLine("Current assignment details:");
            DisplayDetails();

            Console.Write("Enter assignment name: ");
            string detailAssignment = Console.ReadLine();

            Console.Write("Enter description: ");
            string description = Console.ReadLine();

            Console.Write("Enter progress status: ");
            string status = Console.ReadLine();

            string details = description + " - " + status;
            assignmentDetails[detailAssignment] = details;

            Console.WriteLine();
            Console.WriteLine("Updated assignment details:");
            DisplayDetails();
        }

        // ---------- FR-07: Remove by Assignment Name (List and Dictionary) ----------

        static void RemoveAssignmentByName()
        {
            Console.WriteLine("Current assignments:");
            DisplayAssignments();

            Console.Write("Enter the assignment name to remove: ");
            string removeName = Console.ReadLine();

            bool removed = assignments.Remove(removeName);

            if (removed)
                Console.WriteLine("Assignment removed from the list.");
            else
                Console.WriteLine("Assignment not found. List unchanged.");

            Console.WriteLine();
            Console.WriteLine("Updated assignments:");
            DisplayAssignments();
            
            Console.WriteLine();
            Console.Write("Also remove a Dictionary entry? (y/n): ");
            string response = (Console.ReadLine() ?? "").Trim().ToLower();

            if (response == "y")
            {
                Console.WriteLine("Current assignment details:");
                DisplayDetails();

                Console.Write("Enter the Dictionary key to remove: ");
                string keyToRemove = Console.ReadLine();

                if (assignmentDetails.Remove(keyToRemove))
                    Console.WriteLine("Dictionary entry removed.");
                else
                    Console.WriteLine("Dictionary key not found.");

                Console.WriteLine();
                Console.WriteLine("Updated assignment details:");
                DisplayDetails();
            }
        }
        

        static void DisplayCourses()
        {
            if (courses.Length == 0)
            {
                Console.WriteLine("(no courses)");
                return;
            }

            for (int i = 0; i < courses.Length; i++)
            {
                Console.WriteLine(i + ". " + courses[i]);
            }
        }

        static void DisplayAssignments()
        {
            if (assignments.Count == 0)
            {
                Console.WriteLine("(no assignments)");
                return;
            }

            for (int i = 0; i < assignments.Count; i++)
            {
                Console.WriteLine(i + ". " + assignments[i]);
            }
        }

        static void DisplayDetails()
        {
            if (assignmentDetails.Count == 0)
            {
                Console.WriteLine("(no assignment details)");
                return;
            }

            foreach (KeyValuePair<string, string> item in assignmentDetails)
            {
                Console.WriteLine(item.Key + ": " + item.Value);
            }
        }
    }
}