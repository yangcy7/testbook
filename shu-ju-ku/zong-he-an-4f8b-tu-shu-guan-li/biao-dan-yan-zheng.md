# 表单验证

{%ace edit=true, lang='python'%}

     @app.route('/', methods=['get', 'post'])
    def index():
        append_form = Append()
    
        if request.method == 'POST':
            if append_form.validate_on_submit():
                author_name = append_form.au_info.data
                book_name = append_form.bk_info.data
                # 判断数据是否存在
                author = Author.query.filter_by(name=author_name).first()
                if not author:
                    try:
                        # 先添加作者
                        author = Author(name=author_name)
                        db.session.add(author)
                        db.session.commit()
                        # 再添加书籍
                        book = Book(name=book_name, author_id=author.id)
                        db.session.add(book)
                        db.session.commit()
                    except Exception as e:
                        db.session.rollback()
                        print e
                        flash("数据添加错误")
                else:
                    book_names = [book.name for book in author.books]
                    if book_name in book_names:
                        flash('该作者已存在相同的书名')
                    else:
                        try:
                            book = Book(name=book_name, author_id=author.id)
                            db.session.add(book)
                            db.session.commit()
                        except Exception as e:
                            db.session.rollback()
                            print e
                            flash('数据添加错误')
            else:
                flash('数据输入有问题')
    
        authors = Author.query.all()
        books = Book.query.all()
        return render_template('test2.html', authors=authors, books=books, append_form=append_form)
    
{%endace%}

  * 重新编写html文件展示列表书籍

{%ace edit=true, lang='python'%}

    &lt;h2&gt;书籍列表&lt;/h2&gt;
    &lt;ul&gt;
    {% for author in authors %}
        &lt;li&gt;
            {{ author.name }}
            &lt;ul&gt;
                {% for book in author.books %}
                    &lt;li&gt;{{ book.name }}
                {% else %}
                        &lt;li&gt;无书籍&lt;/li&gt;
                {% endfor %}
            &lt;/ul&gt;
    
        &lt;/li&gt;
    {% endfor %}
    &lt;/ul&gt;
    
{%endace%}

  * 在form标签下添加 flash 消息的显示

{%ace edit=true, lang='python'%}

    {% for message in get_flashed_messages() %}
    {{ message }}
    {% endfor %}
    
{%endace%}

____

