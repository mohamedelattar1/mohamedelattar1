        //SICXE Machine Project that get Pass1 Pass2 & HTE Record 



            package sickxeproject12h;
            import java.io.File;
            import java.io.FileNotFoundException;
            import java.io.IOException;
            import java.util.HashMap;
            import java.util.Scanner;

            public class SICKXEProject12h {

            public static int[] locationCounter=new int[1000];
            public static String[] LocatorinHex=new String [1000];
            public static String[] lables = new String [1000];
            public static String[] instractions = new String [1000];
            public static String[] refferance=new String[1000];
            public static String[] objectcode=new String [1000];
            public static String[] address=new String [1000];
            public static String[] disp=new String [1000];
            public static String[] opcode=new String [1000];
            public static final String[][] OPTAB = new String[59][3];
            public static String n,i,x,b,p,e;
            public static String  x1="0",x2="0";
            public static int Base;
            public static int Tag;
            public static String BaseIns;
            public static   int lenghhinD=0;
            public static String []opcodein8=new String [1000];
            public static int instractionCountMax=1;
            public static int instractionCount;
            public static int startLennght;
            public static int pc=0;
            public static int lenghtofinst;
            public static int []Record=new int [1000];
            //split THe File to Arrayes 
            public static void readfile(File input) throws FileNotFoundException,NullPointerException {

                Scanner read=new Scanner(input);
                int i=0;
                while(read.hasNext())
                {
                    String line=read.nextLine();
                    String[] x=line.split(" ");
                    if(x.length == 3)
                    {
                        lables[i]=x[0];
                        instractions[i]=x[1];
                        refferance[i]=x[2];

                    }    
                    else if(x.length==(2))
                    {
                        lables[i]=" ";
                        instractions[i]=x[0]; 
                        refferance[i]=x[1];        
                    }
                    else if(x.length==1){
                        lables[i]=" ";
                        instractions[i]=x[0];
                        refferance[i]=" ";
                    }
                    i++;

                } 
                lenghtofinst=i;
            }
            //Pass 1 ...
            public static int getrefftoDec(String hex){
                String digits="0123456789ABCDEF";
                hex=hex.toUpperCase();
                int val=0;
                for(int i=0 ;i<hex.length();i++){
                    char c=hex.charAt(i);
                    int d=digits.indexOf(c);
                    val=16*val+d;
                }
                return val;
            }
            public static void pass1() throws FileNotFoundException,NullPointerException{
                locationCounter[0]=getrefftoDec(refferance[0]);
                locationCounter[1]=locationCounter[0];
                LocatorinHex[0]=refferance[0];
                LocatorinHex[1]=LocatorinHex[0];
                System.out.println("\t"+locatorin4(LocatorinHex[0])+"\t"+lables[0]+"\t"+instractions[0]+"\t"+refferance[0]);
                int i;
                int y=lenghtofinst;
                //try{
                for(i=1;i<y;i++){
                    if(instractions[i].equalsIgnoreCase("WORD")&&!refferance[i].contains(","))
                    {
                        locationCounter[i+1]=locationCounter[i]+3;
                    }
                    else if (instractions[i].equalsIgnoreCase("WORD")&&refferance[i].contains(",")){
                    String[] x=refferance[i].split(",");
                    locationCounter[i+1]=locationCounter[i]+x.length*3;
                    }

                    else if(instractions[i].equalsIgnoreCase("BASE"))
                    {
                        locationCounter[i+1]=locationCounter[i-1]+searchformat(i-1);
                        locationCounter[i]=0;

                     }
                    else if(instractions[i].equalsIgnoreCase("RESB"))
                    {
                        int main3=Integer.parseInt(refferance[i],10);
                        locationCounter[i+1]=locationCounter[i]+main3;

                    }
                    else if(instractions[i].equalsIgnoreCase("RESW"))
                    {
                        int main4=Integer.parseInt(refferance[i],10);
                        int main6=main4*3;
                        locationCounter[i+1]=locationCounter[i]+main6;
                    }     
                    else if(instractions[i].equalsIgnoreCase("BYTE"))
                    {
                        if(refferance[i].startsWith("C"))
                        {
                            int main6=refferance[i].length()-3;
                            locationCounter[i+1]=locationCounter[i]+main6;
                        }
                        else if(refferance[i].startsWith("X"))
                        {
                            int main8=(refferance[i].length()-3)/2;
                            locationCounter[i+1]=locationCounter[i]+main8; 
                        }
                    }
                    else if(instractions[i].startsWith("+"))
                    {
                        locationCounter[i+1]=locationCounter[i]+4;
                    }        

                    else
                    {
                        locationCounter[i+1]=locationCounter[i]+searchformat(i);
                    } 

                    if(i==1)
                    {
                     System.out.println("\t"+locatorin4(LocatorinHex[i])+"\t"+lables[i]+"\t"+instractions[i]+"\t"+refferance[i]);
                    }
                    else
                    {
                    LocatorinHex[i]=toHex(locationCounter[i]);
                    lenghhinD=locationCounter[i];
                    System.out.println("\t"+locatorin4(LocatorinHex[i])+"\t"+lables[i]+"\t"+instractions[i]+"\t"+refferance[i]);
                    }
                }
                //}
                //catch(Exception e)
                //{ 
                    //System.out.print("Error");
                //}        
            }    

        public static int searchformat(int i){

            for(int j=0;j<OPTAB.length;j++)
                        {
                            if(instractions[i].equals(OPTAB[j][0]))
                            {
                               if(OPTAB[j][1].equalsIgnoreCase("1"))
                                {   
                                   return 1;
                                }
                               else if(OPTAB[j][1].equalsIgnoreCase("2"))
                               {
                                    return 2;
                                }
                               else if(OPTAB[j][1].equalsIgnoreCase("4"))  {
                                   return 4;
                               }
                            }

                        }
                         return 3;  
                        }

        public static String locatorin4(String i){

            String  x=i;
            while(x.length()<4){
                x="0"+x;
            }
            return x;
        }

        public static String toHex(int decimal){    
             int rem;  
             String hex=""; 
             char hexchars[]={'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};  
            while(decimal>0)  
             {  
               rem=decimal%16;   
               hex=hexchars[rem]+hex;   
               decimal=decimal/16;  
             }  
            return hex;  
        }    

            //Simple TABLEEE
            public static void simpletable()throws FileNotFoundException,NullPointerException{
                 System.out.println("\n\t\t\t SYMPOLE TABLE \t\t\t\t\n");
                 int i;
                 try{

                for(i=0;i<lenghtofinst;i++)
                {
                        if(!lables[i].equalsIgnoreCase(" "))
                        {
                        System.out.println("\t\t\t"+locatorin4(LocatorinHex[i])+"\t|\t"+lables[i]);
                        }

                }
                 }
                catch(Exception e){
                        System.out.println("\n");
                       }

            }
            //////////////////////////////////////////////////////////////////////////////////////////

            public static String getOpcodeinBin(String y) {
                String binary = "";
                y = y.toUpperCase();
                HashMap<Character, String> hashMap = new HashMap<Character, String>();
                hashMap.put('0', "0000");
                hashMap.put('1', "0001");
                hashMap.put('2', "0010");
                hashMap.put('3', "0011");
                hashMap.put('4', "0100");
                hashMap.put('5', "0101");
                hashMap.put('6', "0110");
                hashMap.put('7', "0111");
                hashMap.put('8', "1000");
                hashMap.put('9', "1001");
                hashMap.put('A', "1010");
                hashMap.put('B', "1011");
                hashMap.put('C', "1100");
                hashMap.put('D', "1101");
                hashMap.put('E', "1110");
                hashMap.put('F', "1111");
                int i;
                char ch;
                for (i = 0; i < y.length(); i++) {
                    ch = y.charAt(i);
                    if (hashMap.containsKey(ch))

                        binary += hashMap.get(ch);
                    else {
                        binary = "Invalid Hexadecimal String";
                        return binary;
                    }
                }
                return binary;
            }

           //Get Opcode for all program
            public static void getopcode(String y,int i ){


                    for(int j=0;j<OPTAB.length;j++)
                        {

                            if(y.equals(OPTAB[j][0]))       
                            {
                                opcodein8[i]=getOpcodeinBin(OPTAB[j][2]);
                                opcode[i]=opcodein8[i].substring(0,opcodein8[i].length()-2);
                                break;
                            }

                        }
                    }
          //passs2  
            public static void  searchbase(){
                for(int i=0;i<lenghtofinst;i++)
                {
                    if(instractions[i].equalsIgnoreCase("BASE"))
                    {
                        Base=searchTag(refferance[i]);
                        break;
                    }
                }
            }    

            public static void getborp(int y , int x1){
               if( y< 2048 && y> -2048)
               {      
                   p="1";  
                   b="0";

               }
                else 
               {
                    b="1";
                    p="0";
                    searchbase(); 
                    disp[x1]=Integer.toHexString(Tag-Base);


               }
            }
            public static int searchTag(String i){
                int tag=0;

                 for (int x=0;x<lenghtofinst;x++)
                {   
                   if(lables[x].equals(i))
                   {
                       tag=locationCounter[x];
                       break;
                   }
                }

                return tag; 
            } 
            public static void getpc(int x1){
                if(instractions[x1+1].equalsIgnoreCase("BASE")){
                    pc=locationCounter[x1+2];
                }
                else{
                    pc=locationCounter[x1+1];
                }
            }
            public static void getObjectcodeFormat3(int x1 ){
                    getpc(x1);
                    if (refferance[x1].equalsIgnoreCase(" ")){
                        disp[x1]="0";
                        e="0";
                        b="0";
                        p="0";
                        n="1";
                        i="1";
                    }
                     else if (refferance[x1].startsWith("#")&&Character.isDigit(refferance[x1].charAt(1))){
                        n="0";
                        i="1";
                        e="0";
                        x="0";
                        b="0";
                        p="0";
                        disp[x1]=Integer.toHexString(Integer.parseInt(refferance[x1].substring(1,refferance[x1].length())));
              }
                    else if(refferance[x1].startsWith("#")&&!Character.isDigit(refferance[x1].charAt(1))) {
                        n="0";
                        i="1";
                        e="0";
                        x="0";
                        Tag=searchTag(refferance[x1].substring(1));
                        disp[x1]=Integer.toHexString(Tag-pc);
                        getborp(Tag-pc,x1);
                    }
                    else if(refferance[x1].endsWith("X"))
                    {
                        n="1";
                        i="1";
                        e="0";
                        x="1";
                        Tag=searchTag(refferance[x1].substring(0,refferance[x1].length()-2));
                        disp[x1]=Integer.toHexString(Tag-pc);
                        getborp(Tag-pc,x1);
                    }
                   else if(refferance[x1].startsWith("@")){
                        n="1";
                        i="0";
                        e="0";
                        x="0";
                        Tag=searchTag(refferance[x1].substring(1));
                        disp[x1]=Integer.toHexString(Tag-pc);
                        getborp(Tag-pc,x1);
                }
                   else{
                        n="1";
                        i="1";
                        e="0";
                        x="0";
                        Tag=searchTag(refferance[x1]);
                        disp[x1]=Integer.toHexString(Tag-pc);
                        getborp(Tag-pc,x1);
                   }
                   if(disp[x1].length()>3){
                       disp[x1]=disp[x1].substring(disp[x1].length()-3,disp[x1].length());
                   }
                        objectcode[x1]=getOpcodein6(toHex(Integer.parseInt(opcode[x1]+n+i+x+b+p+e,2))+dispin3(disp[x1]));

            }
        public static String dispin3(String i){
            String  x=i;
            while(x.length()<3){
                x="0"+x;
            }
            return x;
        }
        public static String getOpcodein6(String i){
            String  x=i;
            while(x.length()<6){
                x="0"+x;
            }
            return x;
        }

        //object code format 4 
            public static void getPbjectCode(int x1){
               if (refferance[x1].equalsIgnoreCase(" ")){
                    address[x1]=null;
                        e="1";
                        b="0";
                        p="0";
                        n="1";
                        i="1";
               }
               else if (refferance[x1].startsWith("#")&&Character.isDigit(refferance[x1].charAt(1))){
                        address[x1]=toHex(Integer.parseInt(refferance[x1].substring(1,refferance[x1].length())));
                        e="1";
                        b="0";
                        p="0";
                        n="0";
                        i="1";

                    }
              else if(refferance[x1].startsWith("#")&&!Character.isDigit(refferance[x1].charAt(1))) {
                        e="1";
                        b="0";
                        p="0";
                        n="0";
                        i="1";
                        address[x1]=toHex(searchTag(refferance[x1].substring(0)));

                    }
                    else if(refferance[x1].endsWith("X"))
                    {   
                         address[x1]=toHex(searchTag(refferance[x1].substring(0,refferance[x1].length()-2)));
                         x="1";    
                         e="1";
                         p="0";
                         b="0";
                         n="1";
                         i="1";
                    }   
                    else if(refferance[x1].startsWith("@")){
                        address[x1]=toHex(searchTag(refferance[x1]));
                        e="1";
                        b="0";
                        p="0";
                        n="1";
                        i="0";

                    }
                    else{
                        address[x1]=toHex(searchTag(refferance[x1]));
                        e="1";
                        b="0";
                        p="0";
                        n="1";
                        i="1";
                    }
                    objectcode[x1]=toHex(Integer.parseInt(opcode[x1]+n+i+p+b+x+e,2))+addin5(address[x1]);
                }

          public static String addin5(String  i){
            String  x=i;
            while(x.length()<5){
                x="0"+x;
            }
            return x;
          }
            //objectcode format 1 or 2
            public static String getRegisters(String y) {
                String binary = "";
                y = y.toUpperCase();
                HashMap<Character, String> hashMap = new HashMap<Character, String>();
                hashMap.put('A', "0");
                hashMap.put('X', "1");
                hashMap.put('B', "4");
                hashMap.put('S', "5");
                hashMap.put('T', "6");
                hashMap.put('F', "7");
                int i=0;
                char ch;
                ch = y.charAt(i);
                    if (hashMap.containsKey(ch))

                        binary = hashMap.get(ch);
                    else {
                        binary = "Invalid Hexadecimal String";
                        return binary;
                    }
                return binary;
            }
            public static void format1or2(int i){
               if (locationCounter[i+1]-locationCounter[i]==2){

                   if(refferance[i].contains(",")){
                       x1=getRegisters(refferance[i].substring(0));
                       x2=getRegisters(refferance[i].substring(2));

                   }
                   else{
                       x1=getRegisters(refferance[i].substring(0));;
                       x2="0";
                   }
                          objectcode[i]=toHex(Integer.parseInt(opcodein8[i],2))+x1+x2;
               }
               else{
                   objectcode[i]=opcode[i];
               }
            }
            public static void searchObjectCode(int i){
                if(locationCounter[i+1]-locationCounter[i]==4){
                    getPbjectCode(i);
                }
                else if (locationCounter[i+1]-locationCounter[i]==2||locationCounter[i+1]-locationCounter[i]==1)
                {
                   format1or2(i);
                }
                else{
                    getObjectcodeFormat3(i);
                }  
            }
            //PASSS 2 
            public static void pass2()throws FileNotFoundException, IOException{

                int i;
                try{
                    System.out.print("\n\n");
                for(i=0;i<lenghtofinst;i++){
                    if(instractions[i].equalsIgnoreCase("RESB")||instractions[i].equalsIgnoreCase("RESW")
                       ||instractions[i].equalsIgnoreCase("START")||instractions[i].equalsIgnoreCase("END")||instractions[i].equalsIgnoreCase("BASE"))
                    {
                        objectcode[i]="\t";
                        opcode[i]="\t";
                    }
                    else if (instractions[i].equalsIgnoreCase("BYTE")){
                        if(refferance[i].startsWith("C"))
                        {
                            objectcode[i]="";
                            for(int help=2;help<refferance[i].length()-1;help++){
                                char help2=refferance[i].charAt(help);
                                int asci=help2;
                                objectcode[i]+=Integer.toHexString(asci);
                            }

                        }
                        else if(refferance[i].startsWith("X"))
                        {
                            objectcode[i]=refferance[i].substring(2,refferance[i].length()-1);
                        }
                    }
                    else if(instractions[i].equalsIgnoreCase("WORD")&&!refferance[i].contains(",")){
                        objectcode[i]=toHex(Integer.parseInt(refferance[i]));
                    }
                    else if (instractions[i].equalsIgnoreCase("WORD")&&refferance[i].contains(",")){
                        objectcode[i]="";
                        String[] x=refferance[i].split(",");
                        for(int help=0;help<x.length;help++){
                            objectcode[i]+=toHex(Integer.parseInt(x[help]));
                        }
                    }
                    else
                    {
                        if(instractions[i].startsWith("+")){
                        String help=instractions[i].substring(1,instractions[i].length());
                        getopcode(help,i);
                        }
                        else
                        {
                        getopcode(instractions[i],i);
                         }
                        searchObjectCode(i);                
                    }
                    System.out.println("\t"+locatorin4(LocatorinHex[i])+"\t"+lables[i]+"\t"+instractions[i]+"\t"+refferance[i]+"\t"+objectcode[i]);
                }

                }
                catch(Exception e){
                    System.out.print("Error");
                }

            }

        //HTE Record 
            public static String getLenght(){
                for(int i=0;i<lenghtofinst;i++){
                    lenghhinD=locationCounter[i];
                }
                System.out.print(lenghhinD);
                lenghhinD=lenghhinD;
                String lenght=Integer.toHexString(lenghhinD);
                return lenght;
            }
            public static int getLenghtofRecord(){
                int c=0;
                int y=0;
                int lenghtofRecord=0;
                boolean x=true;
                instractionCount=instractionCountMax;
                while(c<30)
                {   
                     if(instractions[instractionCountMax+1].equals("END")){
                      lenghtofRecord=locationCounter[instractionCountMax+1]-startLennght;

                    }
                     else{
                     lenghtofRecord=locationCounter[instractionCountMax]-startLennght;
                     }
                    if(instractions[instractionCountMax].equalsIgnoreCase("RESB")||instractions[instractionCountMax].equalsIgnoreCase("RESB"))
                    {
                        instractionCountMax++;
                        break;
                    }
                    else if (instractions[instractionCountMax+1].equalsIgnoreCase("BASE")){
                         y=(locationCounter[instractionCountMax+2]-locationCounter[instractionCountMax]);
                         c=c+y;
                         if(c>30){
                             break;

                         }
                         else{
                        instractionCountMax++;
                    }
                    }
                    else if (instractions[instractionCountMax].equalsIgnoreCase("BASE")){

                        instractionCountMax++;
                    }
                    else{
                        y=(locationCounter[instractionCountMax+1]-locationCounter[instractionCountMax]);
                        c=c+y;
                        if(c>30){
                            break;
                        }
                        else    
                        {
                        instractionCountMax++;
                        }
                    }

                      x=checkend();
                      if(x==false){
                          break;
                      }
                }
                startLennght=locationCounter[instractionCountMax+1];
                return lenghtofRecord;
            }
            public static void getRecord(){
              while(instractionCount<instractionCountMax)
                {
                    if(instractions[instractionCount].equalsIgnoreCase("BASE")){
                        instractionCount++;
                    }
                    else if (instractions[instractionCount].equalsIgnoreCase("RESB")|instractions[instractionCount].equalsIgnoreCase("RESW")){
                        instractionCount++;
                    }
                    else
                    {
                    System.out.print(""+objectcode[instractionCount]);
                    instractionCount++;
                    }
                }
            }
            public static boolean checkend(){
                if(instractions[instractionCountMax].equals("END"))
                {
                    return false;
                }

                else
                {
                return true;
                }
            }
            public static void HTERecord()throws FileNotFoundException, IOException{
                try{
                    System.out.println("\n\t\t\tHTE Record\n");
                    System.out.println("\tH^"+lables[0]+"^"+getOpcodein6(refferance[0])+"^"+getOpcodein6(Integer.toHexString(lenghhinD-locationCounter[0])));
                    boolean x=true;

                    while(x==true)
                    {
                        startLennght=locationCounter[instractionCount];
                        System.out.print("\tT^"+getOpcodein6(Integer.toHexString(startLennght))+"^"+Integer.toHexString(getLenghtofRecord())+"^");
                        getRecord();
                        x=checkend();
                        System.out.print("\n");
                    }     
                System.out.println("\tE^"+getOpcodein6(refferance[0]));
                }
                catch(Exception e)
                {
                    System.out.print("Error");
                }
            }






            public static void main(String[] args) throws FileNotFoundException, IOException {
                OPTAB[0] = new String[] {"FIX", "1", "C4"};
                OPTAB[1] = new String[] {"FLOAT", "1", "C0"};
                OPTAB[2] = new String[] {"HIO", "1", "F4"};
                OPTAB[3] = new String[] {"NORM", "1", "C8"};
                OPTAB[4] = new String[] {"SIO", "1", "F0"};
                OPTAB[5] = new String[] {"TIO", "1", "F8"};
                OPTAB[6] = new String[] {"ADDR", "2", "90"};
                OPTAB[7] = new String[] {"CLEAR", "2", "B4"};
                OPTAB[8] = new String[] {"COMPR", "2", "A0"};
                OPTAB[9] = new String[] {"DIVR", "2", "9C"};
                OPTAB[10] = new String[] {"MULR", "2", "98"};
                OPTAB[11] = new String[] {"RMO", "2", "AC"};
                OPTAB[12] = new String[] {"SHIFTL", "2", "A4"};
                OPTAB[13] = new String[] {"SHIFTR", "2", "A8"};
                OPTAB[14] = new String[] {"SUBR", "2", "94"};
                OPTAB[15] = new String[] {"SVC", "2", "B0"};
                OPTAB[16] = new String[] {"TIXR", "2", "B8"};
                OPTAB[17] = new String[] {"ADD", "3", "18"};
                OPTAB[18] = new String[] {"ADDF", "3", "58"};
                OPTAB[19] = new String[] {"AND", "3", "40"};
                OPTAB[20] = new String[] {"COMP", "3", "28"};
                OPTAB[21] = new String[] {"COMPF", "3", "88"};
                OPTAB[22] = new String[] {"DIV", "3", "24"};
                OPTAB[23] = new String[] {"DIVF", "3", "64"};
                OPTAB[24] = new String[] {"J", "3", "3C"};
                OPTAB[25] = new String[] {"JEQ", "3", "30"};
                OPTAB[26] = new String[] {"JGT", "3", "34"};
                OPTAB[27] = new String[] {"JLT", "3", "38"};
                OPTAB[28] = new String[] {"JSUB", "3", "48"};
                OPTAB[29] = new String[] {"LDA", "3", "00"};
                OPTAB[30] = new String[] {"LDB", "3", "68"};
                OPTAB[31] = new String[] {"LDCH", "3", "50"};
                OPTAB[32] = new String[] {"LDF", "3", "70"};
                OPTAB[33] = new String[] {"LDL", "3", "08"};
                OPTAB[34] = new String[] {"LDS", "3", "6C"};
                OPTAB[35] = new String[] {"LDT", "3", "74"};
                OPTAB[36] = new String[] {"LDX", "3", "04"};
                OPTAB[37] = new String[] {"LPS", "3", "D0"};
                OPTAB[38] = new String[] {"MUL", "3", "20"};
                OPTAB[39] = new String[] {"MULF", "3", "60"};
                OPTAB[40] = new String[] {"OR", "3", "44"};
                OPTAB[41] = new String[] {"RD", "3", "D8"};
                OPTAB[42] = new String[] {"RSUB", "3", "4C"};
                OPTAB[43] = new String[] {"SSK", "3", "EC"};
                OPTAB[44] = new String[] {"STA", "3", "0C"};
                OPTAB[45] = new String[] {"STB", "3", "78"};
                OPTAB[46] = new String[] {"STCH", "3", "54"};
                OPTAB[47] = new String[] {"STF", "3", "80"};
                OPTAB[48] = new String[] {"STI", "3", "D4"};
                OPTAB[49] = new String[] {"STL", "3", "14"};
                OPTAB[50] = new String[] {"STS", "3", "7C"};
                OPTAB[51] = new String[] {"STSW", "3", "E8"};
                OPTAB[52] = new String[] {"STT", "3", "84"};
                OPTAB[53] = new String[] {"STX", "3", "10"};
                OPTAB[54] = new String[] {"SUB", "3", "1C"};
                OPTAB[55] = new String[] {"SUBF", "3", "5C"};
                OPTAB[56] = new String[] {"TD", "3", "E0"};
                OPTAB[57] = new String[] {"TIX", "3", "2C"};
                OPTAB[58] = new String[] {"WD", "3", "DC"};
                File input=new File("");//link of your file ; 
                readfile(input);
                pass1();
                simpletable();
                pass2();
                HTERecord();
                System.out.println("\t\t\t|The Program Has Finished ^_*|");
            }

        }
