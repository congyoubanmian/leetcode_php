336. 回文对
给定一组唯一的单词， 找出所有不同 的索引对(i, j)，使得列表中的两个单词， words[i] + words[j] ，可拼接成回文串。

示例 1:

输入: ["abcd","dcba","lls","s","sssll"]
输出: [[0,1],[1,0],[3,2],[2,4]] 
解释: 可拼接成的回文串为 ["dcbaabcd","abcddcba","slls","llssssll"]
示例 2:

输入: ["bat","tab","cat"]
输出: [[0,1],[1,0]] 
解释: 可拼接成的回文串为 ["battab","tabbat"]

来源：力扣（LeetCode）
https://leetcode-cn.com/problems/palindrome-pairs/

class Solution {

    /**
     * @param String[] $words
     * @return Integer[][]
     */
    function palindromePairs($words) {
        $n = count($words);
        $trie = new Trie;
        for ($i = 0; $i < $n; $i++) {
            $trie->insert($words[$i], $i);
        }
        $ret = [];
        for ($i = 0; $i < $n; $i++) {
            $m = strlen($words[$i]);
            for ($j = 0; $j <= $m; $j++) {
                if ($this->isPalindrome($words[$i], $j, $m - 1)) {
                    $other_left = $trie->findWord($words[$i], 0, $j - 1);
                    if ($other_left != -1 && $other_left != $i) {
                        $array = [$i, $other_left];
                        array_push($ret, $array);
                    }
                }
                if ($j != 0 && $this->isPalindrome($words[$i], 0, $j - 1)) {
                    $other_right = $trie->findWord($words[$i], $j, $m - 1);
                    if ($other_right != -1 && $other_right != $i) {
                        $array = [$other_right, $i];
                        array_push($ret, $array);
                    }
                }
            }
        }
        return $ret;

    }

    public function isPalindrome($s, $left, $right) {
        $len = $right - $left + 1;
        for ($i = 0; $i < $len / 2; $i++) {
            if ($s[$left + $i] != $s[$right - $i]) {
                return false;
            }
        }
        return true;
    }

}
class TrieNode
{
    public $char;
    public $is_end = -1;
    public $children;

    public function __construct()
    {
        $this->children = [];
    }
}
class Trie
{
    public $root;
    /**
     * Initialize your data structure here.
     */
    public function __construct()
    {
        $this->root = new TrieNode();
    }

    /**
     * Inserts a word into the trie.
     * @param String $word
     * @return NULL
     */
    public function insert($word, $index)
    {
        $root   = $this->root;
        for ($i = 0; $i < strlen($word); $i++) {
            if (!isset($root->children[$word[$i]])) {
                $root->children[$word[$i]]       = new TrieNode();
                $root->children[$word[$i]]->char = $word[$i];
            }
            $root = $root->children[$word[$i]];
        }
        $root->is_end = $index;
    }

    /**
     * Returns if the word is in the trie.
     * @param String $word
     * @return Boolean
     */
    public function findWord($s, $left, $right)
    {
        $root = $this->root;
        for ($i = $right; $i >= $left; $i--) {
            $x = $s[$i];
            if (!isset($root->children[$x])) {
                return -1;
            }
            $root = $root->children[$x];
        }
        // apple
        return $root->is_end;
    }

}