package artpricingsystem;

import java.io.File;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.Date;


public class AuctionPainting extends Painting
{

    private Date dateOfAuction;
    private double auctionSalesPrice;

    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting 
    AuctionPainting (String fname, String lname, 
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
    AuctionPainting()
    {
        artistFirstName="";
	artistLastName="";
	titleOfWork="";
	dateOfWork=new Date();
	dateOfAuction =new Date();
        auctionSalesPrice =0;
	medium="";
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
    
    //Desc: updates the dateOfPurchase in a record from user's input
    //Post: dateOfPurchase field is updated 
    public void updateDateofAuction()
    {
        try 
        {
            System.out.println("Old Date of Purchase:" + dateOfAuction);
            System.out.println("Please enter new Date of Purchase (mm/dd/yyyy) and press <ENTER>: ");
            Date tempDate=new Date(UserInterface.getString());
            dateOfAuction=tempDate;
        }
        
        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not a date value. Please enter a date value (mm/dd/yyyy): "); 
           Date tempDate=new Date (UserInterface.getString());
           dateOfAuction=tempDate;
           return;
        }
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

        //Desc: updates the dateOfPurchase in a record from user's input
    //Post: dateOfPurchase field is updated 
    public void updateAuctionSalesPrice()
    {
        try 
        {
            System.out.println("Old Target Selling Price:" + auctionSalesPrice);
            System.out.println("Please enter new Target Selling Price and press <ENTER>: ");
            double tempprice=new Double(UserInterface.getString());
            auctionSalesPrice=tempprice;
        }
        
        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not an double value. Please enter a double value: "); 
           auctionSalesPrice=Double.parseDouble(UserInterface.getString());
           return;
        }
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
            System.out.println ("***** Error: AuctionPainting.find () *****");
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
	//Auction Sales Price
	input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Double price = new Double(input.toString());
        auctionSalesPrice = price;
        i++;
        //Medium
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
        System.out.println ("***** Error: AuctionPainting.read () *****");
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
        fileName.writeBytes(medium + "|" + "\n");
    }
    catch (IOException e)
    {
        System.out.println ("***** Error: AuctionPainting.write () *****");
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
            AuctionPainting tempAuct = new AuctionPainting ();  
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
                while (oldFile.getFilePointer () != oldFile.length ()) 
                {
                    tempAuct.read(oldFile); 
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
            } 
            newFile.close ();
            auctFile.delete ();
            tempAuctFile.renameTo (auctFile);
            System.out.println("record saved to file");
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.putRecord () *****");
            System.out.println ("\t" + e);
        }
    }
    public void print () 
    { 
        System.out.print ("Artist First Name: " + artistFirstName);
        System.out.print ("\t Artist Last Name: " + artistLastName); 
        System.out.println ("\t Title of Work: " + titleOfWork); 
        System.out.println ("\t Date of Work: " + dateOfWork); 
        System.out.println ("\t Date of Auction: " + dateOfAuction); 
        System.out.println ("\t Auction Sales Price: " + auctionSalesPrice); 
        System.out.println ("\t Medium: " + medium); 
    } 
    
    //Desc: reads in all fields into a Painting object from user input
    //Post: all fields in Auction Painting are modified
    public void readInRecord()
    {
        try
        {         
            System.out.println("Enter Artist First name: ");
            artistFirstName = UserInterface.getString();
            while(artistFirstName.length()>31) 
            {
                System.out.println("Artist First Name exceeds 30 characters. Please enter shortened name:");
                artistFirstName=UserInterface.getString();
            }               
            
            System.out.println("Enter Artist Last name: ");
            artistLastName= UserInterface.getString();
            while(artistLastName.length()>31) 
            {
                System.out.println("Artist Last Name exceeds 30 characters. Please enter shortened name:");
                artistLastName=UserInterface.getString();
            }            
            
            System.out.println("Enter title of painting: ");
            titleOfWork = UserInterface.getString();
            while(titleOfWork.length()>41) 
            {
                System.out.println("Title of Work exceeds 40 characters. Please enter shortened name:");
                titleOfWork=UserInterface.getString();
            }    
            
            System.out.println("Enter the date the painting was created (mm/dd/yyyy): ");
            Date tempdate = new Date(UserInterface.getString());
            dateOfWork=tempdate; 
            
            System.out.println("Enter the date the painting was auctioned (mm/dd/yyyy): ");
            Date tempauctdate = new Date(UserInterface.getString());
            dateOfWork=tempauctdate;            
            
            System.out.println("Enter painting medium (oil, watercolor, or other): ");
            medium  = UserInterface.getString();
            while (!(medium.equalsIgnoreCase("oil")|medium.equalsIgnoreCase("watercolor")|
                    medium.equalsIgnoreCase("other")))
            {
                System.out.println("Medium entered incorrectly. Please enter one of the following mediums: oil, watercolor, or other.");
                medium=UserInterface.getString();
            }
            
            System.out.println("Enter painting's auction Sale Price: ");
            Double tempw=new Double( UserInterface.getString());
            auctionSalesPrice =tempw;
        }
        
        catch (Exception e)
        {
           System.out.println("Record value entered is not the correct data type. Please re-enter the record: "); 
           readInRecord();
        }
        /* catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.readInRecord () *****");
            System.out.println ("\t" + e);
        }*/
    }
    
    //Desc: Deletes a single record in the GalleryPaintings.dat File if the artist last 
    //      name and first name matches this object.
    //Post: Modifies the GalleryPaintings.dat File   
    public void performDeletion ()
    {
        try
        {
            File  paintingsFile = new File ("AuctionPainting.dat");
            File  tempPaintingsFile = new File ("AuctionPainting.tmp");

            AuctionPainting ap = new AuctionPainting ();	// record to be checked

            if (!paintingsFile.exists ())
            {
              return;
            }

            RandomAccessFile inFile = new RandomAccessFile (paintingsFile, "r");
            RandomAccessFile outFile = new RandomAccessFile (tempPaintingsFile, "rw");

            while (inFile.getFilePointer () != inFile.length ())
            {
              ap.read (inFile);

              if (!(artistLastName.equalsIgnoreCase(ap.getArtistLastName()) &&
                        titleOfWork.equalsIgnoreCase(ap.getTitleofWork())))
              {
                  ap.write (outFile);
              }
            }

            inFile.close ();
            outFile.close ();

            paintingsFile.delete ();
            tempPaintingsFile.renameTo (paintingsFile);

        }
        catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.performDeletion () *****");
            System.out.println ("\t" + e);
        }
    }  
    
    public double findPrice(String alastname,  String med, String sub, double area)
    {
        try
        {
            System.out.println("start");
            DetermineMostSimilarWork simwork = new DetermineMostSimilarWork();
            File paintingsFile = new File ("AuctionPainting.dat");
            boolean found = false;
            double max=0;
            double coeff=0;
            double dummycoeff=0;
            int subjectnumber=0;
            int mediumnumber=0;
            if (paintingsFile.exists())
            {
                System.out.println("file exists");
                RandomAccessFile inFile = new RandomAccessFile (paintingsFile, "r");
                while (!found && (inFile.getFilePointer()!=inFile.length()))
                {
                    read (inFile);
                    if (artistLastName.equalsIgnoreCase(alastname) )
                    {
                        System.out.println("found artist");
                        if (subject.equalsIgnoreCase(sub))
                             subjectnumber=1;
                        else subjectnumber=0;


                        if(medium.equalsIgnoreCase(med))
                            mediumnumber=1;
                        else mediumnumber=0;

                        double largerArea=simwork.compareReturnLarger(width*height, area);
                        double smallerArea=simwork.compareReturnSmaller(width*height, area);
                        dummycoeff=(mediumnumber+subjectnumber)*smallerArea/largerArea;
                        System.out.println(dummycoeff);
                        if(dummycoeff>coeff && (inFile.getFilePointer()==inFile.length()))
                        {
                            found = true;
                            max=auctionSalesPrice;
                        }
                        else if (dummycoeff>coeff && (inFile.getFilePointer()!=inFile.length()))
                            max=auctionSalesPrice;
                    }
                }
                inFile.close();

            }
          /*  if (coeff==0)
            {
                System.out.println("The coefficient of similarity is zero for this artist.");
                return 0;
            }*/
            return max;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: BoughtPainting.findPrice () *****");
            System.out.println ("\t" + e);
            return 0;
        }
    }
}


