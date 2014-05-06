package artpricingsystem;
import java.io.*;
import java.util.Scanner;


 //tested: gets, sets, constructors, find, read, updates (with new UI method), write
//Fixes: save doesn't save new record yet

class Artist {

private String artistFirstName;
private String artistLastName ;
private int fashionabilityValue;

    //Desc: constructor for Artist
    //Post: allows class to set the value of the artistFirstName, artistLastName,
    //		and fashionabilityValue field in a record
    Artist(String f, String l, int v)
    {
        artistFirstName=f;
        artistLastName=l;
        fashionabilityValue=v;
    }

    Artist() {
    }

    //Desc: allows class to access the artistFirstName field in a record
    //Return: artistFirstName
    public String getArtistFirstName()
    {
        return artistFirstName;
    }
	
    //Desc:allows class to set  value of the artistFirstName field in a record
    //Post: artistFirstName is set to f
    public void setArtistFirstName(String f)
    {
        artistFirstName=f;
    }

    //Desc: allows class to access the artistLastName field in a record
    //Return: artistLastName	
    public String getArtistLastName()
    {
        return artistLastName;
    }

    //Desc:allows class to set the value of the artistLastName field in a record
    //Post: dateOfPurchase is set to l
    public void setArtistLastName(String l)
    {
        artistLastName=l;
    } 

    //Desc: allows class to access the fashionabilityValue field in a record
    //Return: fashionabilityValue	
    public int getArtistFashionabilityValue()
    {
        return fashionabilityValue;
    }

    //Desc:allows class to set  value of fashionabilityValue field in a record
    //Post: dateOfPurchase is set to v
    public void setArtistFashionabilityValue(int v) 
    {
        fashionabilityValue=v;
    }

    //Desc: updates an artist's first name in a record from user's input
    //Post: artistsFirstName field is updated 
    public void updateArtistsFirstName()
    {
        String str="";
        System.out.println("Old Artist First Name:" + artistFirstName);
        System.out.println("Please enter new Artist First Name and press <ENTER>: \n");
        artistFirstName=UserInterface.getString();
    }

    //Desc: Finds the record the user wants to update and updates the 
    //artistLastName field in the found object
    //Post: artistLastName field is updated
    public void updateArtistLastName()
    {
        System.out.println("Old Artist Last Name:" + artistLastName);
        System.out.println("Please enter new Artist Last Name");
        artistLastName=UserInterface.getString();
    }	

    //Desc: Finds the record the user wants to update and updates the 
    //fashionabilityValue field in the found object
    //Post: fashionabilityValue field is updated
    public void updateFashionabilityValue()
    {
        System.out.println("Old Fashionability Value:" + fashionabilityValue);
        System.out.println("Please enter new Fashionability Value");
        fashionabilityValue=UserInterface.getInt();
    }	

    //Desc:uses the last name and the first name of an artist to find a record
    //     in the file
    //Post: 
    //Return: returns true if record is found or false if record is not found
    public boolean find(String afirstname, String alastname) //I don't think this works
    {
  // find locates a given investment record if it exists.
  // Returns true if the investment is located, otherwise false.
        try
        {
            File artistFile = new File ("artist.dat");
            boolean found = false;
            if (artistFile.exists())
            {
                RandomAccessFile inFile = new RandomAccessFile (artistFile, "r");
                while (!found && (inFile.getFilePointer()!=inFile.length()))
                {
                    read (inFile);
                    if (artistLastName.equalsIgnoreCase(alastname) && 
                    artistFirstName.equalsIgnoreCase(afirstname))
                        found = true;       
                }
                inFile.close();
            }
            return found;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Investment.find () *****");
            System.out.println ("\t" + e);
            return false; //returns boolean right now, not artist
        }
  }
    
    //Desc: reads an artist record form fileName
    //Pre: file must exist 
    //Post: artistLastName, artistFirstName and fashionabilityValue in this 
    //      object are changed
    public void read(RandomAccessFile fileName)
    {
        try
        {
            String  inputString = new String ();	
            int	i = 0;		                
            inputString = fileName.readLine ();
            StringBuffer input = new StringBuffer ();	
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistFirstName = input.toString();
            i++;
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistLastName = input.toString ();
            i++;
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            Integer tempInt = new Integer (input.toString ());
            fashionabilityValue = tempInt;
            i++;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Arist.read () *****");
            System.out.println ("\t" + e);
        }

    }

    //Desc: writes the variables artistFirstName, artistLastName,
    //		and fashionabilityValue to a record line in the specified file
    //Post: updates the specified file
    public void write(RandomAccessFile fileName)
    {
        try
        {
            fileName.writeBytes(artistFirstName + "|");
            fileName.writeBytes(artistLastName + "|");
            String fashValue="";
            fashValue=fashValue.valueOf(fashionabilityValue);
            fileName.writeBytes(fashValue+"|" + "\n");
        }
        catch (IOException e)
        {
            System.out.println ("***** Error: Artist.write () *****");
            System.out.println ("\t" + e);
        }
    }
 
    //Desc: saves an individual artist record into a file
    //Post: the message informing the user has been printed
    public void save()
    {
        try
        {
            File artistFile = new File ("artist.dat");	
            File  tempArtistFile = new File ("artist.tmp");	
            Artist tempArtist = new Artist ();	
            boolean found = false;		
            RandomAccessFile newFile = new RandomAccessFile (tempArtistFile, "rw");

            if (!artistFile.exists ())
            {
              write(newFile);
            }
            else
            {
                RandomAccessFile oldFile = new RandomAccessFile (artistFile, "r");
                boolean compareArtist;
                while (oldFile.getFilePointer () != oldFile.length ()) //the pointer hasn't reached the end of the file
                {
                    tempArtist.read(oldFile); //read walks through each field in the record

                    System.out.println(artistFirstName.equalsIgnoreCase(tempArtist.getArtistFirstName()));
                    System.out.println(artistLastName.equalsIgnoreCase(tempArtist.getArtistLastName()));
                    if (artistFirstName.equalsIgnoreCase(tempArtist.getArtistFirstName()) &&
                    artistLastName.equalsIgnoreCase(tempArtist.getArtistLastName()))
                        compareArtist=true;
                    else compareArtist=false;
                    if(compareArtist) 
                    {
                        write (newFile); 
                        found=true;
                    } 
                    else
                    {
                      tempArtist.write(newFile); 
                    }
                }  
                if (!found) write (newFile); // if never found in file, write to temp file

              oldFile.close ();

            }  // else

            newFile.close ();

            artistFile.delete ();
            tempArtistFile.renameTo (artistFile);
            System.out.println("record saved to file");

          }
          catch (Exception e)
          {
              System.out.println ("***** Error: Artist.putRecord () *****");
              System.out.println ("\t" + e);
          }
      }
  public void print ()
  //
  // displays the contents of an investment object.
  //
  {
      System.out.print ("Artist First Name: " + artistFirstName);
      System.out.print ("\t Artist Last Name: " + artistLastName);
      System.out.println ("\t Fashionability Value: " + fashionabilityValue);
  }  // print

}
