require(['gitbook', 'jQuery'], function (gitbook, $) {
	var TOGGLE_CLASSNAME = 'expanded',
		CHAPTER = '.chapter',
		ARTICLES = '.articles',
		TRIGGER_TEMPLATE = '<i class="exc-trigger fa"></i>',
		LS_NAMESPACE = 'expChapters';
	var init = function () {
		// adding the trigger element to each ARTICLES parent and binding the event
		// 将触发器添加到每个articles父元素并绑定事件
		$(ARTICLES)
			.parent(CHAPTER)
			.children('a, span')
			.on('click', function (e) {
				// e.preventDefault();
				// e.stopPropagation();
				toggle($(e.target).parent(CHAPTER));
			})
			.append(
				$(TRIGGER_TEMPLATE)
				// .on('click', function(e) {
				// e.preventDefault();
				// e.stopPropagation();
				//   toggle($(e.target).closest(CHAPTER));
				// })
			);
		expand(lsItem());
		// expand current selected chapter with it's parents
		// 展开当前选定的章节与它的父节点
		var activeChapter = $(CHAPTER + '.active');
		expand(activeChapter);
		expand(activeChapter.parents(CHAPTER));
		// 如果只含有隐藏章节，则隐藏展开图片
		let hideDom = $('span:contains($hide$)');
		hideDom.parent().addClass('hide');
		hideDom
			.parent()
			.parent()
			.each((idx, el) => {
				let flag = [];
				$(el)
					.children('li')
					.each((idx, li_el) => {
						$(li_el).hasClass('hide') ? flag.push(true) : flag.push(false);
					});
				console.log(flag);
				if (!flag.includes(false)) {
					$(el).parent(CHAPTER).children('a, span').children('.fa').css({ display: 'none' });
				}
			});
	};
	var toggle = function ($chapter) {
		if ($chapter.hasClass('expanded')) {
			collapse($chapter);
		} else {
			expand($chapter);
		}
	};
	var collapse = function ($chapter) {
		if ($chapter.length && $chapter.hasClass(TOGGLE_CLASSNAME)) {
			$chapter.removeClass(TOGGLE_CLASSNAME);
			lsItem($chapter);
		}
	};
	var expand = function ($chapter) {
		if ($chapter.length && !$chapter.hasClass(TOGGLE_CLASSNAME)) {
			$chapter.addClass(TOGGLE_CLASSNAME);
			lsItem($chapter);
		}
	};
	var lsItem = function () {
		var map = JSON.parse(localStorage.getItem(LS_NAMESPACE)) || {};
		if (arguments.length) {
			var $chapters = arguments[0];
			$chapters.each(function (index, element) {
				var level = $(this).data('level');
				var value = $(this).hasClass(TOGGLE_CLASSNAME);
				map[level] = value;
			});
			localStorage.setItem(LS_NAMESPACE, JSON.stringify(map));
		} else {
			return $(CHAPTER).map(function (index, element) {
				if (map[$(this).data('level')]) {
					return this;
				}
			});
		}
	};

	gitbook.events.bind('page.change', function () {
		init();
	});
});
