# 删除数据

  * 定义删除author和book的路由

{%ace edit=true, lang='python'%}

    # 删除作者
    @app.route('/delete_author/&lt;int:author_id&gt;')
    def delete_author(author_id):
        author = Author.query.get(author_id)
        if not author:
            flash('数据不存在')
        else:
            try:
                Book.query.filter_by(author_id=author_id).delete()
                db.session.delete(author)
                db.session.commit()
            except Exception as e:
                db.session.rollback()
                print e
                flash('操作数据库失败')
    
        return redirect(url_for('index'))
    
    
    # 删除书籍
    @app.route('/delete_book/&lt;int:book_id&gt;')
    def delete_book(book_id):
        book = Book.query.get(book_id)
        if not book:
            flash('数据不存在')
        else:
            try:
                db.session.delete(book)
                db.session.commit()
            except Exception as e:
                db.session.rollback()
                print e
                flash('操作数据库失败')
    
        return redirect(url_for('index'))
    
{%endace%}

  * 在模版中添加删除的 a 标签链接

{%ace edit=true, lang='python'%}

    &lt;h2&gt;书籍列表&lt;/h2&gt;
    &lt;ul&gt;
    {% for author in authors %}
        &lt;li&gt;
            {{ author.name }} &lt;a href="/delete_author/{{ author.id }}"&gt;删除&lt;/a&gt;
            &lt;ul&gt;
                {% for book in author.books %}
                    &lt;li&gt;{{ book.name }} &lt;a href="/delete_book/{{ book.id }}"&gt;删除&lt;/a&gt;&lt;/li&gt;
                {% else %}
                        &lt;li&gt;无书籍&lt;/li&gt;
                {% endfor %}
            &lt;/ul&gt;
    
        &lt;/li&gt;
    {% endfor %}
    &lt;/ul&gt;
    
{%endace%}

____

