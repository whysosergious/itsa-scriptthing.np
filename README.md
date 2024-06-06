# testdrive_n_rnd.np
testdrive and rnd  related to scripting, shellservers and template compiler generaators.



### ffs


// $nu - closely refer to nushells functions, structures & so on. if similar logic exists in try to adopt it as close to nushels behaviour.
//
// $c - holds the underlying/invisible sprecial variables like $in, $it or maybe any context data?
// the 'c' in my head stands for context or caret. as it will both hold our stuff aswell as always follow/control the position and direction of a process.
// ```js
// instdead of `left -> | -> right` piping direction. we'll do
`top'
  ⇓
  -  
  ⇓
'down`
// maybe we should just try a couple of different things and see if we can focus on actually transpiling ts_script to nuscript


// lets give it a try, first nu:
'http get htt


// ```
// [ eg: $c.in]


const get_readme = (repo) => https://raw.githubusercontent.com/($repo)/main/README.md ;

  get_readme('akinsho/toggleterm.nvim') ;
  
  $nu.lines ;
  
  $nu.each((ln) => {
  
})


// ----

// nu-script

'akinsho/toggleterm.nvim' $"http://akinsho/toggleterm.nvim/($in).md/main/readme.md" | http get $in |
  lines | each { |ln| $"--##($ln)" } | ast-grep --##```$$$--##``` | each { |m| str replace "--##" "    " |  } 

// the above pipe should give us an array where every row that is not part of a code snippet is 
// prepended with --## (-- comment string in lua) as well as extra indent @ the snippets
// which should make it very easy to mark with tree-sitter for i.e folding. how would a ts pipe look?

// ts-script . note: the $c object doesn't meed to be visible, I'll elaborate soon.
// also the '$nu' object simply hold references to nu's logic. 

'akinsho/toggleterm.nvim' ;
  /*-*/
`http://akinsho/toggleterm.nvim/${$in}.md/main/readme.md` ;
  /*-*/
await fetch($in) ;
  /*-*/
$nu.lines ; // lines in ts would simply be str.split('\n'), but we can run in nu too..
  /*-*/
$nu.each(($ln) => `--##${$ln}\n`) ;
  /* - ast-grep in ts ? yup ! and we can alway fallback to regexp */
new AstGrep('ast-grep --##```$$$--##```') 
  /*-*/  
$nu.each(($m) => JSON.stringify($m).replace('--#', '    ').split('\n'))
ast()

// and we should have an identical return here.
// THOOOUGH .. it doesn't seem to be valid js from a to z so are we meant to Jit every pipe-step into nuon and back?
// weeeell .. here's where the 'offside buffer comes in, the 'invisible' $c I meantioned earlier.
// it simply follows us around, thus context/caret are both not incorrect names here. e.g:

// the data-struct is a basic array of expressions
// $c runs each line as a function, and stores the result in its 'in' property (
// the rest is only made better! if we store as much data as we are able. passing on the $in prop as the pipe-step result,
// we get the ability to time travel!!! and even branch out basically indefinetely 😬 

$c.pipe(($in) => {
  // nested pipe
  $c.in = 
})

$c  /*-*/ ($in) => {
              'akinsho/toggleterm.nvim' ;
              $c.in = '${$in}' ;
          }
$c.ag_pttrn0 = new AstGrep('--##```$$$--##```') ; 
$c.tmp0 = $c.ag_pttrn0.match($c.in) ;
$c.in = $c.tmp0 ;
$c.tmo0 = $c.in.join('\n') ;
$c.in = $c.tmp0 ;
$c    /*-*/ ($in) => {
              // ast grep works with the ast so we always have structured data .. our matches are the same type as what we matched it with (array of lines)
              // I also see now that I didn't need to do arr -> str -> rgx.replace -> str.split
              // we'll just do: match of matches then change each row
  const $match in $m /*$matches*/ ;
$c  /*-*/ ($in) => { 
              //// so yeah ...
              //// I think we can traspile, compile pr just Jit all over 🥴
