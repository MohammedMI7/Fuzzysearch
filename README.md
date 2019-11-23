// Fuzzysearch
//Fuzzy search using levenshtein algortihm
package fuzzysearch;
import fuzzysearch.Levenshtein2.Searcher;
import java.io.File;
import java.io.FileNotFoundException;
import java.util.*;
import java.util.logging.Level;
import java.util.logging.Logger;

class Levenshtein2{
    private int[][] wordMartix;

    public Set similarExists(String searchWord) {

        int maxDistance = searchWord.length();
        int curDistance;
        int sumCurMax;
        String checkWord;

        // preventing double words on returning list
        Set<String> fuzzyWordList = new HashSet<>();

        for (Object wordList : Searcher.wordList) {
            checkWord = String.valueOf(wordList);
            curDistance = calculateDistance(searchWord, checkWord);
            sumCurMax = maxDistance + curDistance;
            if (sumCurMax == checkWord.length()) {
                fuzzyWordList.add(checkWord);
            }
        }
        return fuzzyWordList;
    }

    public int calculateDistance(String inputWord, String checkWord) {
        wordMartix = new int[inputWord.length() + 1][checkWord.length() + 1];

        for (int i = 0; i <= inputWord.length(); i++) {
            wordMartix[i][0] = i;
        }

        for (int j = 0; j <= checkWord.length(); j++) {
            wordMartix[0][j] = j;
        }

        for (int i = 1; i < wordMartix.length; i++) {
            for (int j = 1; j < wordMartix[i].length; j++) {
                if (inputWord.charAt(i - 1) == checkWord.charAt(j - 1)) {
                    wordMartix[i][j] = wordMartix[i - 1][j - 1];
                } else {
                    int minimum = Integer.MAX_VALUE;
                    if ((wordMartix[i - 1][j]) + 1 < minimum) {
                        minimum = (wordMartix[i - 1][j]) + 1;
                    }
                    if ((wordMartix[i][j - 1]) + 1 < minimum) {
                        minimum = (wordMartix[i][j - 1]) + 1;
                    }

                    if ((wordMartix[i - 1][j - 1]) + 1 < minimum) {
                        minimum = (wordMartix[i - 1][j - 1]) + 1;
                    }

                    wordMartix[i][j] = minimum;
                }
            }
        }

        return wordMartix[inputWord.length()][checkWord.length()];
    }

    public static class Searcher {

        private static ArrayList<String> wordList;
        
        Searcher(){
            wordList = new ArrayList();
            try(Scanner s=new Scanner(new File("E:\\Java\\sample.txt")).useDelimiter("\\s* \\s*")){
            while(s.hasNext()){
                wordList.add(s.next());
            }
        } catch (FileNotFoundException ex) {
            Logger.getLogger(Fuzzysearch.class.getName()).log(Level.SEVERE, null, ex);
        }
        }

    }

}
public class Fuzzysearch{
public static void main(String[] args){
    Levenshtein2 obj=new Levenshtein2();
    Searcher sr = new Searcher();
    Scanner sc=new Scanner(System.in);
    System.out.print("Enter a word to search:");
    String str=sc.next();
    System.out.print(obj.similarExists(str)+"\n");
   
}
}
