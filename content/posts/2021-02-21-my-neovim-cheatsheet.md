---
title: My (Neo)Vim Cheatsheet
author: lopeztel
date: 2021-02-21T16:32:14+00:00
url: /2021/02/21/my-neovim-cheatsheet/
tags:
  - 100DaysToOffload
  - Neovim
  - Tutorial

---
I&#8217;ve been learning to use neovim for almost 3 weeks now and it has been fun, coming from VSCode I guess I was spoiled by the feature rich IDE/text editor, however there is beauty in the simplicity keyboard oriented (Neo)vim text editor

I&#8217;ve been trying to get all the useful keyboard shortcuts and commands I&#8217;ve found in a single place and I though this could be helpful for other noobs like me, [fosstodon](https://fosstodon.org) community was helpful and shared their Tips n&#8217; Tricks as well.

## The Cheatsheet {#the-cheatsheet}

This list is by no means a complete compendium but it is what I&#8217;ve found most useful and basic

  * To enter **NORMAL** mode press `Esc`
  * To enter **INSERT** mode type `i` (cursor position) or `I` (beginning of the current line)

From **NORMAL** mode:

  * To enter **VISUAL** mode type `v`
  * To enter **VISUAL LINE** mode type `V`
  * To enter **VISUAL BLOCK** mode press `Ctrl + v`
  * To append text type `a` (cursor position) or `A` (end of the current line)
  * To navigate the document left, down, up and right press `h`, `j`, `k`, `l` respectively (or use the arrow keys)
  * To go to the top and end of the document, type `gg` and `G` respectively
  * To move to the beginning and end of the line type `` and `$` respectively
  * To move forward or backward one word press `w` and `b` respectively
  * To insert a line below or above the current line type `o` and `O` respectively
  * To undo type `u`, to redo press `Ctrl + r`
  * To repeat an action press `.`
  * To move between last opened files press `Ctrl + i` (newer) `Ctrl + o` (older)
  * To swap lines type `ddp`
  * To swap two characters type `xp`
  * To remove the current character type `x`
  * To replace the current character type `r<character>`
  * To correct a word (from the cursor position) type `cw`
  * To correct a word (regardless of the cursor position) type `ciw`
  * To scroll down the document so that only the last line is visible type `Ctrl + f`
  * To search the document type `/<search term>`
  * To go to next and previous tabs type `gt` and `gT`
  * To switch between splits press `Ctrl + w <movement keys>` or `Ctrl + w + w`
  * To start recording a macro press `q<letter>`, then execute actions/commands and pres `q` to stop recording
  * To execute a macro type `@<letter>`, to execute the macro again type `@@`
  * To enter **COMMAND** mode type `:`

From **COMMAND** mode:

  * To close a file type`q`
  * To close a file discarding any changes type `q!`
  * To close the text editor and all opened files type `qa`
  * To save your changes to a file type `w`
  * To save your changes and quit all open tabs `wqa`
  * To read the help documentation about a topic type `help <topic string>`
  * To read the help documentation about commands associated to a key type `help <key>`
  * To open a new tab type `tabnew <optional file>`
  * To switch between opened tabs type `tabprevious` and `tabnext` respectively
  * To open a new horizontal split type `split <optional file>`
  * To open a new vertical split type `vsplit <optional file>`
  * To search and replace globally type `%s/<regex or search term>/<replace term>/g<optional nc for confirmation>`
  * To &#8220;open a terminal&#8221; type `term`

From **VISUAL LINE** mode:

  * To reflow text type `gq`

Additional resources:

  * [Using Macros in Vim](https://slashdev.space/posts/2021-01-26-vim-macros), by [@Garrit][1]
  * [Strategies to use a terminal alongside (Neo)Vim](https://slashdev.space/posts/2021-02-24-vim-terminal-strategies) by [@Garrit](https://fosstodon.org/@garritfra)
  * [A great Vim Cheat Sheet](https://vimsheet.com/)

## My current configuration {#my-current-configuration}

I thought that just for fun I&#8217;d share my `init.vim` (as of now, since I am still tweaking it) in case someone finds it useful:

```vim
" Author: Lopeztel
" Description: Lopeztel's nvim config
" Last Modified: February 20, 2021

" Startify
let g:startify_custom_header= [
\ '                      _                         _       _                      ', 
\ '                     | |    ___  _ __   ___ ___| |_ ___| |                     ',
\ '                     | |   / _ \| ._ \ / _ \_  / __/ _ \ |                     ',
\ '                     | |__| (_) | |_) |  __// /| ||  __/ |                     ',
\ '                     |_____\___/| .__/ \___/___|\__\___|_|                     ',
\ '                                |_|                                            ',
\ '                                                                               ',
\ ]
call plug#begin('~/.vim/plugged')

Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'arcticicestudio/nord-vim'
Plug 'vim-airline/vim-airline'
Plug 'tpope/vim-fugitive'
Plug 'tpope/vim-rhubarb'
Plug 'junegunn/gv.vim'
Plug 'honza/vim-snippets'
Plug 'junegunn/fzf'
Plug 'junegunn/fzf.vim'
Plug 'mhinz/vim-startify'
Plug 'godlygeek/tabular'
Plug 'plasticboy/vim-markdown'
Plug 'rhysd/vim-clang-format'
Plug 'preservim/nerdcommenter'
" Plug 'liuchengxu/vim-which-key'

call plug#end()

" Set colorscheme
colorscheme nord

" vim-airline settings
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#formatter = 'unique_tail'
let g:airline_powerline_fonts = 1
" let g:Powerline_symbols = 'fancy'
 
" Map the leader key to SPACE
let mapleader="\<SPACE>"

" set line numbers 
set number
" More natural splits
set splitbelow          " Horizontal split below current.
set splitright          " Vertical split to right of current.

" Set spell check to English
 autocmd FileType markdown setlocal spell spelllang=en_us

" Configuration for vim-markdown
let g:vim_markdown_folding_disabled = 1
let g:vim_markdown_math = 1
let g:vim_markdown_toml_frontmatter = 1
let g:vim_markdown_frontmatter = 1
let g:vim_markdown_strikethrough = 1
let g:vim_markdown_autowrite = 1
let g:vim_markdown_edit_url_in = 'tab'
let g:vim_markdown_follow_anchor = 1

" TextEdit might fail if hidden is not set.
set hidden

" Some servers have issues with backup files, see #649.
set nobackup
set nowritebackup

" Give more space for displaying messages.
set cmdheight=2

" Having longer updatetime (default is 4000 ms = 4 s) leads to noticeable
" delays and poor user experience.
set updatetime=300

" Don't pass messages to |ins-completion-menu|.
set shortmess+=c

" Always show the signcolumn, otherwise it would shift the text each time
" diagnostics appear/become resolved.
if has("patch-8.1.1564")
  " Recently vim can merge signcolumn and number column into one
  set signcolumn=number
else
  set signcolumn=yes
endif

" Use tab for trigger completion with characters ahead and navigate.
" NOTE: Use command ':verbose imap <tab>' to make sure tab is not mapped by
" other plugin before putting this into your config.
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" Use <c-space> to trigger completion.
if has('nvim')
  inoremap <silent><expr> <c-space> coc#refresh()
else
  inoremap <silent><expr> <c-@> coc#refresh()
endif

" Make <CR> auto-select the first completion item and notify coc.nvim to
" format on enter, <cr> could be remapped by other vim plugin
inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm()
                              \: "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"

" Use `[g` and `]g` to navigate diagnostics
" Use `:CocDiagnostics` to get all diagnostics of current buffer in location list.
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)

" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Use K to show documentation in preview window.
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  elseif (coc#rpc#ready())
    call CocActionAsync('doHover')
  else
    execute '!' . &keywordprg . " " . expand('<cword>')
  endif
endfunction

" Highlight the symbol and its references when holding the cursor.
autocmd CursorHold * silent call CocActionAsync('highlight')

" Symbol renaming.
nmap <leader>rn <Plug>(coc-rename)

" Formatting selected code.
xmap <leader>f  <Plug>(coc-format-selected)
nmap <leader>f  <Plug>(coc-format-selected)

augroup mygroup
  autocmd!
  " Setup formatexpr specified filetype(s).
  autocmd FileType typescript,json setl formatexpr=CocAction('formatSelected')
  " Update signature help on jump placeholder.
  autocmd User CocJumpPlaceholder call CocActionAsync('showSignatureHelp')
augroup end

" Applying codeAction to the selected region.
" Example: `<leader>aap` for current paragraph
xmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)

" Remap keys for applying codeAction to the current buffer.
nmap <leader>ac  <Plug>(coc-codeaction)
" Apply AutoFix to problem on the current line.
nmap <leader>qf  <Plug>(coc-fix-current)

" Map function and class text objects
" NOTE: Requires 'textDocument.documentSymbol' support from the language server.
xmap if <Plug>(coc-funcobj-i)
omap if <Plug>(coc-funcobj-i)
xmap af <Plug>(coc-funcobj-a)
omap af <Plug>(coc-funcobj-a)
xmap ic <Plug>(coc-classobj-i)
omap ic <Plug>(coc-classobj-i)
xmap ac <Plug>(coc-classobj-a)
omap ac <Plug>(coc-classobj-a)

" Remap <C-f> and <C-b> for scroll float windows/popups.
if has('nvim-0.4.0') || has('patch-8.2.0750')
  nnoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? coc#float#scroll(1) : "\<C-f>"
  nnoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? coc#float#scroll(0) : "\<C-b>"
  inoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? "\<c-r>=coc#float#scroll(1)\<cr>" : "\<Right>"
  inoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? "\<c-r>=coc#float#scroll(0)\<cr>" : "\<Left>"
  vnoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? coc#float#scroll(1) : "\<C-f>"
  vnoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? coc#float#scroll(0) : "\<C-b>"
endif

" Use CTRL-S for selections ranges.
" Requires 'textDocument/selectionRange' support of language server.
nmap <silent> <C-s> <Plug>(coc-range-select)
xmap <silent> <C-s> <Plug>(coc-range-select)

" Add `:Format` command to format current buffer.
command! -nargs=0 Format :call CocAction('format')

" Add `:Fold` command to fold current buffer.
command! -nargs=? Fold :call     CocAction('fold', <f-args>)

" Add `:OR` command for organize imports of the current buffer.
command! -nargs=0 OR   :call     CocAction('runCommand', 'editor.action.organizeImport')

" Add (Neo)Vim's native statusline support.
" NOTE: Please see `:h coc-status` for integrations with external plugins that
" provide custom statusline: lightline.vim, vim-airline.
set statusline^=%{coc#status()}%{get(b:,'coc_current_function','')}

" Mappings for CoCList
" Show all diagnostics.
nnoremap <silent><nowait> <space>a  :<C-u>CocList diagnostics<cr>
" Manage extensions.
nnoremap <silent><nowait> <space>e  :<C-u>CocList extensions<cr>
" Show commands.
nnoremap <silent><nowait> <space>c  :<C-u>CocList commands<cr>
" Find symbol of current document.
nnoremap <silent><nowait> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols.
nnoremap <silent><nowait> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent><nowait> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent><nowait> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list.
nnoremap <silent><nowait> <space>p  :<C-u>CocListResume<CR>

" Coc-explorer
let g:coc_explorer_global_presets = {
\   '.vim': {
\     'root-uri': '~/.vim',
\   },
\   'tab': {
\     'position': 'tab',
\     'quit-on-open': v:true,
\   },
\   'floating': {
\     'position': 'floating',
\     'open-action-strategy': 'sourceWindow',
\   },
\   'floatingTop': {
\     'position': 'floating',
\     'floating-position': 'center-top',
\     'open-action-strategy': 'sourceWindow',
\   },
\   'floatingLeftside': {
\     'position': 'floating',
\     'floating-position': 'left-center',
\     'floating-width': 50,
\     'open-action-strategy': 'sourceWindow',
\   },
\   'floatingRightside': {
\     'position': 'floating',
\     'floating-position': 'right-center',
\     'floating-width': 50,
\     'open-action-strategy': 'sourceWindow',
\   },
\   'simplify': {
\     'file-child-template': '[selection | clip | 1] [indent][icon | 1] [filename omitCenter 1]'
\   }
\ }

nmap <space>e :CocCommand explorer<CR>
nmap <space>ef :CocCommand explorer --preset floating<CR>
autocmd BufEnter * if (winnr("$") == 1 && &filetype == 'coc-explorer') | q | endif

" clang-format configuration
let g:clang_format#style_options = {
            \ "AccessModifierOffset" : -4,
            \ "AllowShortIfStatementsOnASingleLine" : "true",
            \ "AlwaysBreakTemplateDeclarations" : "true",
            \ "Standard" : "C++11"}

" map to <Leader>cf in C++ code
autocmd FileType c,cpp,objc nnoremap <buffer><Leader>cf :<C-u>ClangFormat<CR>
autocmd FileType c,cpp,objc vnoremap <buffer><Leader>cf :ClangFormat<CR>
" Toggle auto formatting:
nmap <Leader>C :ClangFormatAutoToggle<CR>

" Nerd Commenter
" " Use compact syntax for prettified multi-line comments
let g:NERDCompactSexyComs = 1
" " Allow commenting and inverting empty lines (useful when commenting a region)
let g:NERDCommentEmptyLines = 1
" " Enable trimming of trailing whitespace when uncommenting
let g:NERDTrimTrailingWhitespace = 1
" " Enable NERDCommenterToggle to check all selected lines is commented or not
let g:NERDToggleCheckAllLines = 1
" " Add spaces after comment delimiters by default
let g:NERDSpaceDelims = 1

vmap <Leader>/ <plug>NERDCommenterToggle
nmap <Leader>/ <plug>NERDCommenterToggle
```

---

Day 51 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!

 [1]: https://fosstodon.org/@garritfra
