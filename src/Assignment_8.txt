// this program takes the characteristics of one
// person and compares them to another such that
// of a dating profile system

public class Foothill 
{
   // main method takes profiles and prints the results
   public static void main(String[] args)
   {
      // four different candidates
      DateProfile person1 = new DateProfile('M','F',7,9,"Michael");
      DateProfile person2 = new DateProfile('M','F',9,3,"Jim");
      DateProfile person3 = new DateProfile('F','M',9,4,"Pam");
      DateProfile person4 = new DateProfile('F','M',8,1,"Phyllis");

      // person 1 compared to all 4 profiles
      displayTwoProfiles(person1, person1);
      displayTwoProfiles(person1, person2);
      displayTwoProfiles(person1, person3);
      displayTwoProfiles(person1, person4);
      System.out.println();

      // person 2 compared to all 4 profiles
      displayTwoProfiles(person2, person1);
      displayTwoProfiles(person2, person2);
      displayTwoProfiles(person2, person3);
      displayTwoProfiles(person2, person4);
      System.out.println();

      // person 3 compared to all 4 profiles
      displayTwoProfiles(person3, person1);
      displayTwoProfiles(person3, person2);
      displayTwoProfiles(person3, person3);
      displayTwoProfiles(person3, person4);
      System.out.println();

      // person 4 compared to all 4 profiles
      displayTwoProfiles(person4, person1);
      displayTwoProfiles(person4, person2);
      displayTwoProfiles(person4, person3);
      displayTwoProfiles(person4, person4);
   }
   
   // prints the formatted compatibility between two people
   static void displayTwoProfiles
   (DateProfile profile1, DateProfile profile2)
   {
      System.out.println("The compatibility between " + 
         profile1.getName() + " and " + profile2.getName() + 
            ": " + profile1.fitValue(profile2));
   }
}

// contains all mutator and accessor methods
class DateProfile 
{
   // default values for profiles
   private static final char DEFAULT_GEN = 'M';
   private static final char DEFAULT_SEARCH_GEND = 'F';
   private static final String DEFAULT_NAME= "N/A";
   private static final int DEFAULT_ROMANCE= 5;
   private static final int DEFAULT_FINANCE= 5;
   
   // max and min for values
   public static final int MIN_ROMANCE = 1;
   public static final int MAX_ROMANCE = 10;
   public static final int MIN_FINANCE = 1;
   public static final int MAX_FINANCE = 10;

   // values to be taken into account
   private char gender;
   private char searchGender;
   private int romance;
   private int finance;
   private String name;

   // default constructor with no parameters
   public DateProfile()
   {
      setDefaults();
   }
   
   // sets values for default constructor
   void setDefaults()
   {
      setGender(DEFAULT_GEN);
      setSearchGender(DEFAULT_SEARCH_GEND);
      setRomance(DEFAULT_ROMANCE);
      setFinance(DEFAULT_FINANCE);
      setName(DEFAULT_NAME);
   }

   // constructor with parameters
   public DateProfile(char gender, char searchGender, 
         int romance, int finance, String name)
   {
      setAll(gender, searchGender, romance, finance, name);
   }
   
   // sets values for constructor with parameters
   void setAll(char gender, char searchGender,
         int romance, int finance, String name)
   {
      setGender(gender);
      setSearchGender(searchGender);
      setRomance(romance);
      setFinance(finance);
      setName(name);
   }
   
   // checks/sets valid gender, if not sets to default
   public boolean setGender(char gender) 
   {
      if(gender == 'M' || gender =='F')
      {
         this.gender = gender;
         return true;
      }
      else
      {
         this.gender = DEFAULT_GEN;
         return false;
      }
   }

   // checks/sets valid search gender, if not sets to default
   public boolean setSearchGender(char searchGender) 
   {
      if(searchGender == 'M' || searchGender =='F')
      {
         this.searchGender = searchGender;
         return true;
      }
      else
      {
         this.searchGender = DEFAULT_SEARCH_GEND;
         return false;
      }
   }

   // checks/sets valid romance value, if not sets to default
   public boolean setRomance(int romance) 
   {
      if(romance <= MAX_ROMANCE && romance >= MIN_ROMANCE)
      {
         this.romance = romance;
         return true;
      }
      else
      {
         this.romance = DEFAULT_ROMANCE;
         return false;
      }
   }

   // checks/sets valid finance value, if not sets to default
   public boolean setFinance(int finance) 
   {
      if(finance <= MAX_FINANCE && finance >= MIN_FINANCE)
      {
         this.finance = finance;
         return true;
      }
      else
      {
         this.finance = DEFAULT_FINANCE;
         return false;
      }
   }

   // checks/sets valid name, if not sets to default
   public boolean setName(String name) 
   {
      if(name.length() >= 0 )
      {
         this.name = name;
         return true;
      }
      else
      {
         this.name = DEFAULT_NAME;
         return false;
      }
   }
   
   // returns value
   public char getGender()
   {
      return gender;
   }
   
   public char getSearchGender() 
   {
      return searchGender;
   }
   
   public int getRomance() 
   {
      return romance;
   }
   
   public int getFinance() 
   {
      return finance;
   }
   
   public String getName() 
   {
      return name;
   }

   // checks if search gender is compatible
   private double determineGenderFit(DateProfile partner)
   {
      if(getSearchGender() == partner.getGender())
      {
         return 1;
      }
      else
      {
         return 0;
      }
   }

   // determines romance ratio 
   private double determineRomanceFit(DateProfile partner)
   {
      int difference = Math.abs(getRomance() - partner.getRomance());

      // same romance level
      if(difference == 0)
      {
         return 1;
      }
      else
      {
         int maxDifference = MAX_ROMANCE - MIN_ROMANCE;
         double ratio = ((double)
               ( maxDifference - difference )) / maxDifference;

         if(ratio == 0)
         {
            ratio = 0.1;
         }
         return ratio;
      }
   }

   // determines finance ratio
   private double determineFinanceFit(DateProfile partner)
   {
      int difference = Math.abs(getFinance() - partner.getFinance());

      // same finance value
      if(difference == 0)
      {
         return 1;
      }
      else
      {
         int maxDifference = MAX_FINANCE - MIN_FINANCE;
         double ratio = ((double)
               ( maxDifference - difference )) / maxDifference;

         if(ratio == 0)
         {
            ratio = 0.1;
         }
         return ratio;
      }
   }
   
   // calculates overall compatibility
   public double fitValue(DateProfile partner)
   {
      // if wrong search gender, compatibility is zero
      if(determineGenderFit(partner) == 0)
      {
         return 0;
      }
      else
      {
         // sum of ratios determine fit
         double total = determineFinanceFit(partner)
               +determineRomanceFit(partner);
         return (total / 2);
      }
   }
}

/*
\\\\\\\\\\\\\\\\\\\ PASTED OUTPUT /////////////////////////
The compatibility between Michael and Michael: 0.0
The compatibility between Michael and Jim: 0.0
The compatibility between Michael and Pam: 0.6111111111111112
The compatibility between Michael and Phyllis: 0.5

The compatibility between Jim and Michael: 0.0
The compatibility between Jim and Jim: 0.0
The compatibility between Jim and Pam: 0.9444444444444444
The compatibility between Jim and Phyllis: 0.8333333333333333

The compatibility between Pam and Michael: 0.6111111111111112
The compatibility between Pam and Jim: 0.9444444444444444
The compatibility between Pam and Pam: 0.0
The compatibility between Pam and Phyllis: 0.0

The compatibility between Phyllis and Michael: 0.5
The compatibility between Phyllis and Jim: 0.8333333333333333
The compatibility between Phyllis and Pam: 0.0
The compatibility between Phyllis and Phyllis: 0.0
*/

