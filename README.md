# seo-project

def scrape_to_word(url, output_file):

    response = requests.get(url)
    response.raise_for_status()
    soup = BeautifulSoup(response.content, 'html.parser')

    meta_tags = soup.find_all('meta')
    meta_info = []
    for tag in meta_tags:
        attrs = {k: v for k, v in tag.attrs.items()}
        meta_info.append(attrs)

    text = soup.get_text()

    doc = Document()

    doc.add_heading('Meta Tags', level=1)
    table = doc.add_table(rows=1, cols=2)
    hdr_cells = table.rows[0].cells
    hdr_cells[0].text = 'Attribute'
    hdr_cells[1].text = 'Value'
    
    for meta in meta_info:
        row_cells = table.add_row().cells
        for key, value in meta.items():
            row_cells[0].text = key
            row_cells[1].text = value

    doc.add_heading('Page Content', level=1)
    doc.add_paragraph(soup.get_text())

    doc.save(output_file)


url = 'https://www.nerdybirdy.org/'
output_file = 'scraped_content_with_meta_table.docx'
scrape_to_word(url, output_file)

print(f"Content has been saved to {output_file}")
