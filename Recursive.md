# Bonfire: No repeats please
Return the number of total permutations of the provided string that don't have repeated consecutive letters. 
For example, 'aab' should return 2 because it has 6 total permutations, but only 2 of them don't have the same letter (in this case 'a') repeating.
## Representation of input
Because we are interested only in the number of total permutations of some string we must group the possible strings in categories. 'aabb' is in the same category as 'abab' or 'cffc', thus these strings are in the category `(2,2)` because the string is comprised of 2 identical letters and another 2 letters with the same value, no matter in what order are these 4 letters.
This category can be noted as (2^2) too. This 2nd form I will use and I will represent it as an array `c=[,0,2]`. `c[0]==undefined` (or whatever) for my array to begin with index 1 and means that I have `0 of ones and 2 of twos`.

*More examples:*
- 'aaa' as (3) or (3^1)=`[,0,0,1]`,
- 'abcdefa' as (1,1,1,1,1,2) or (1^5,2^1)=`[,5,1]`
- 'abfdefa': (1,1,1,2,2) or `[,3,2]`


## Recursive way
```javascript
function permRec(category, restriction){
  var v,sum=0;
  var c=category;
  if(impossiblePerm(c)) return 0;
  for(var i=c.length-1; i>0; i--){
    v=c[i];
    c[i]--;c[i-1]++;
    sum+=(i==restriction?v-1:v)*i*permRec(c,i-1);
    //restore c
    c[i-1]--;c[i]++;
  }
  return sum;
}
```
### Explanation
`restriction=0` means no restriction in choosing the first letter as from the beginning, which means that I can choose any letter. After I made the choice, the permutation of the remaining letters is restricted to not start with a letter like the previous. The argument ` restriction` is the number of letters equal to chosen letter which has remained after this choice.

