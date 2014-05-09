MasterpieceAuctionPainting
==========================
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.Date;


public class MasterpieceAuctionPainting extends Painting
{

    private Date dateOfAuction;
    private double auctionSalesPrice;

    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting 
    MasterpieceAuctionPainting (String fname, String lname, 
                String titleofWork, Date doWork, Date doAuction,
                double aucSalesPrice, String med)
    {
        artistFirstName=fname;
	artistLastName=lname;
	titleOfWork=titleofWork;
	dateOfWork=doWork;
	dateOfAuction =doAuction;
        auctionSalesPrice = aucSalesPrice;
	medium=med;
    }
    
    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting 
    MasterpieceAuctionPainting()
    {
        
    }

    //Return: The date of the Auction
    public Date getDateofAuction()
    {
        return dateOfAuction;
    }
    
    //Desc: Set date of auction to d
    public void setDateofAuction(Date d)
    {
        dateOfAuction = d;
    }
	//Desc: Set Auction Sales Price to d
    public void setAuctionSalesPrice(double d)
    {
        auctionSalesPrice = d;
    }

    //Return: The auction sales price for the painting
    public double getAuctionSalesPrice()
    {
    	return auctionSalesPrice;
    }

    //Desc: uses the last name of an artist and the title of work to find the
    //       Bought Painting object in the array 
    //Return: returns the found Artist object or null value if Artist not found
    public boolean find(String alastname, String title) //should only appear in painting
    {
        try
        {
            File paintingsFile = new File ("AuctionPaintings.dat");
            boolean found = false;      

            if (paintingsFile.exists ())
            {
                RandomAccessFile inFile = new RandomAccessFile (paintingsFile, "r");

                while (!found && (inFile.getFilePointer () != inFile.length ()))
                {
                    read (inFile);

                    if (artistLastName.equals(alastname) && titleOfWork.equals(title))
                        found = true;
                }
                inFile.close();
            }
            return found;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: MasterpieceAuctionPainting.find () *****");
            System.out.println ("\t" + e);

            return false; //returns boolean right now, not artist
        }
    }
    
//Desc: reads an MasterpieceAuctionPainting record form fileName
//Pre: file must exist 
//Post: all fields in this object are changed
public void read(RandomAccessFile fileName)
{
    try
    {
        String  inputString = new String ();    
        int i = 0;                      
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
        titleOfWork=input.toString();
        i++;
        input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Date tempDate = new Date(input.toString());
        dateOfWork = tempDate;
        i++;
        input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Date tempDateA = new Date(input.toString());
        dateOfAuction=tempDateA;
        i++;
	
	input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Double price = new Double(input.toString());
        auctionSalesPrice = price;
        i++;
        input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        medium = input.toString ();
        i++;
    }
    catch (Exception e)
    {
        System.out.println ("***** Error: Arist.read () *****");
        System.out.println ("\t" + e);
    }

}

//Desc: writes the fields of the object to a record line in the specified file
//Post: updates the specified file
public void write(RandomAccessFile fileName)
{
    try
    {
        fileName.writeBytes(artistFirstName + "|");
        fileName.writeBytes(artistLastName + "|");
        fileName.writeBytes(titleOfWork + "|");
        String dateOfW="";
        dateOfW=dateOfW.valueOf(dateOfWork);
        fileName.writeBytes(dateOfW+"|");
        String dateOfA="";
        dateOfA=dateOfA.valueOf(dateOfAuction);
        fileName.writeBytes(dateOfA+"|");
        String price="";
        price=price.valueOf(auctionSalesPrice);
        fileName.writeBytes(price+"|");
        fileName.writeBytes(medium + "|");
    }
    catch (IOException e)
    {
        System.out.println ("***** Error: MasterpieceAuctionPainting.write () *****");
        System.out.println ("\t" + e);
    }
}

//Desc: saves an individual masterpieceAuctionPainting record into a file
//Post: the message informing the user has been printed
public void save()
{
    try
    {
        File auctFile = new File ("AuctionPainting.dat");  
        File  tempAuctFile = new File ("AuctionPainting.tmp"); 
        MasterpieceAuctionPainting tempAuct = new MasterpieceAuctionPainting ();  
        boolean found = false;      
        RandomAccessFile newFile = new RandomAccessFile (tempAuctFile, "rw");

        if (!auctFile.exists ())
        {
          write(newFile);
        }
        else
        {
            RandomAccessFile oldFile = new RandomAccessFile (auctFile, "r");
            boolean compareAuct;
            while (oldFile.getFilePointer () != oldFile.length ()) //the pointer hasn't reached the end of the file
            {
                tempAuct.read(oldFile); //read walks through each field in the record

                System.out.println(artistFirstName.equalsIgnoreCase(tempAuct.getArtistsFirstName()));
                System.out.println(titleOfWork.equalsIgnoreCase(tempAuct.getTitleofWork()));
                if (artistFirstName.equalsIgnoreCase(tempAuct.getArtistsFirstName()) &&
                titleOfWork.equalsIgnoreCase(tempAuct.getTitleofWork()))
                    compareAuct=true;
                else compareAuct=false;
                if(compareAuct) 
                {
                    write (newFile); 
                    found=true;
                } 
                else
                {
                  tempAuct.write(newFile); 
                }
            }  
            if (!found) write (newFile); // if never found in file, write to temp file

          oldFile.close ();

        }  // else

        newFile.close ();

        auctFile.delete ();
        tempAuctFile.renameTo (auctFile);
        System.out.println("record saved to file");

      }
      catch (Exception e)
      {
          System.out.println ("***** Error: MasterpieceAuctionPainting.putRecord () *****");
          System.out.println ("\t" + e);
      }
  }
    public void print () 
 // displays the contents of an investment object. // 
    { 
        System.out.print ("Artist First Name: " + artistFirstName);
        System.out.print ("\t Artist Last Name: " + artistLastName); 
        System.out.println ("\t Title of Work: " + titleOfWork); 
        System.out.println ("\t Date of Work: " + dateOfWork); 
        System.out.println ("\t Date of Auction: " + dateOfAuction); 
        System.out.println ("\t Auction Sales Price: " + auctionSalesPrice); 
        System.out.println ("\t Medium: " + medium); 
    } // print
    public void performDeletion()
    {
    
    }
    
    public void readInRecord()
    {
        
    }
    

}


